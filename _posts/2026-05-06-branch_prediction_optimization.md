---
title: 부등호가 쏘아 올린 성능 최적화 (3) - 예측을 돕는 코드, 분기를 줄이는 코드
date: 2026-05-06 00:00:00 +0900
description: 코드 한 줄로 성능을 2배 향상시켰다고!!?
categories: [CS, OS]
tags: [cs, os]     # TAG names should always be lowercase
pin: false
math: true
mermaid: true
image:
  path: /assets/img/20260506/branch_prediction_optimization/thumbnail.png
  alt: simulation
  show_in_content: true
---





### 시리즈 흐름

- [1편: 명령어 파이프라인](../instruction_pipeline)
- [2편: 분기 예측](../branch_prediction)

이전 포스팅에서는 CPU 파이프라인과 분기 예측을 살펴보았습니다.

### CPU가 예측하기 쉬운 흐름 만들기

이번 글에서는 정렬, Lookup Table, Branchless 코드를 예제로 살펴보며 분기 예측 실패를 줄이거나 조건 분기 자체를 줄이는 흐름을 확인해 봅니다.

#### 1. 데이터 정렬 (Sorting)

정렬된 데이터는 CPU에게 **'단순한 패턴'**을 제공합니다. 덕분에 분기 예측 적중률이 상승합니다.

##### 🛑 Before: 무작위 데이터 (**22.9ms**)

불규칙한 패턴으로 잦은 예측 실패로 파이프라인이 계속 멈춥니다.

```java
// data: [0, 2, 1, 3, 0, ...] (Random)
for (int v : data) {
    if (v < 2) sum += v; 
}
```

##### ✅ After: 정렬된 데이터 (**8.8ms** 정렬 비용 제외)

정렬된 패턴이 예측 적중률을 높여, 파이프라인이 멈춤 없이 작동합니다

```java
Arrays.sort(data);

// data: [0, 0, 1, 1, 2, ...] (Sorted)
for (int v : data) {
    if (v < 2) sum += v;
}
```

##### ⚠️ 핵심: 언제 정렬해야 할까?

주의할 점은 정렬 비용 $O(n \log n)$이 분기 예측 실패 비용$O(n)$보다 비싸다는 것입니다. 정렬보다는 상황에 따라 자료구조(`PriorityQueue`)나 DB Index 같은 효율적인 대안을 고려해볼 수 있습니다.

#### 2. Lookup Table: 인덱스로 값 찾기
**분기(Branch) 대신 메모리에 직접 접근**하여 성능을 높입니다.



##### 🛑 Before: `switch`로 분기하기 (**43.0ms**)

CPU는 매 루프마다 다음 분기를 예측합니다.

```java
for (int v : data) {
    switch (v) {
        case 0: sum += 1; break;
        case 1: sum += 3; break;
        case 2: sum += 10; break;
    }
}
```



##### ✅ After: Lookup Table로 값 찾기 (**3.4ms**)

분기 없이 주소값을 계산해 값을 가져옵니다.

```java
int[] table = {1, 3, 10};

for (int v : data) {
    sum += table[v]; 
}
```



#### 3. Branchless: 조건문 대신 연산 사용

조건(`if`)에 따른 실행 흐름을 나누는 대신, 연산 결과로 값을 선택하는 방식입니다. 데이터가 불규칙해 분기 예측이 자주 빗나가는 경우에는 이런 형태가 유리할 수 있습니다. 분기 실패로 버려지는 작업이 줄어들 수 있어 CPU 파이프라인 효율을 높일 수 있습니다.

##### 🛑 Before: Branch 방식 (**31.0ms**)

조건에 따라 실행 흐름이 끊깁니다. 데이터가 불규칙하면 예측 실패 비용이 발생합니다.

```java
// 절댓값 구하기
for (int v : data) {
    if (v < 0) sum += -v;
    else sum += v;
}
```

##### ✅ After: Branchless 방식 (**4.6ms**)

분기 없이 일정한 속도로 작업을 처리합니다.

비트 연산이나 삼항 연산자를 활용하여 분기 없이 흐름을 유지합니다.

```java
// if문 삭제 (비트 연산 활용)
for (int v : data) {
    int mask = v >> 31;
    sum += (v + mask) ^ mask;
}
```

##### CMOV(Conditional Move) 원리

조건문이 값 선택 형태로 바뀌면, 어셈블리어로 변환되는 과정에서 **CMOV(Conditional Move)** 같은 명령으로 낮아질 수 있습니다.

###### 1. 어셈블리어 레벨에서의 차이

| **구분**   | **Branch (Jump)**                  | **Branchless (CMOV)**                 |
| ---------- | ---------------------------------- | ------------------------------------- |
| **코드**   | `if (v > 0) sum += v;`             | `sum += (v > 0) ? v : 0;`             |
| **명령어** | `JLE` (Jump Less Equal)            | `CMOVG` (Conditional Move)            |
| **동작**   | 조건에 맞는 코드를 찾아 **Jump**함 | Jump 없이 **조건에 맞는 값만 선택함** |
| **리스크** | 예측 실패 시 파이프라인 중단       | 예측 실패로 인한 중단(Flush) 없음     |



###### 2. 왜 CMOV가 유리한가? (Core Logic)

핵심은 **불확실한 '예측'을 없애고, 확정된 '연산'을 하는 것**입니다.

- **if - Jump**
  
  - **예측 의존:** 다음 실행할 코드를 **추측(Prediction)**하여 미리 가져옵니다.
  - **결과:** 예측 실패 시 파이프라인을 비우는(Flush) 비용이 발생합니다.
  
- **삼항 연산 - CMOV**
  
  - **예측 제거:** 실행 흐름을 나누는 분기 대신, 조건에 맞는 값을 선택하는 형태로 처리될 수 있습니다.
  
  - **결과**: 분기 예측 실패로 인한 흔들림을 줄일 수 있습니다.
  
     

> #### 💡 결론: 데이터 패턴에 따른 최적화 전략
>
> > **🚀 규칙적인 패턴 (Sorted/Predictable)**
> >
> > 패턴이 규칙적이라면 CPU가 미리 처리해둔 파이프라인 병렬 연산이 그대로 유효합니다. 예측 실패로 버려지는 작업이 적기 때문에, **if 문(Jump)**이 효율적입니다.
>
> 
>
> > **🛡️ 불규칙한 패턴 (Random/Unpredictable)**
> >
> > 예측 실패로 인한 **파이프라인 플러시(Flush) 리스크**를 제거해야 하므로, 플러시를 최소화하기 위한 **Branchless (CMOV)**가 효율적입니다.



### 3. 내장 함수 활용

JVM이 자주 사용되는 연산들을 최적화하여 제공하는 방식입니다. 일반적인 메소드 호출 과정을 거치지 않고, OS와 H/W의 특성을 고려해 저수준에서 처리하기 때문에 일반 연산보다 더 빠른 성능을 기대할 수 있습니다.

`@IntrinsicCandidate`는 JVM이 해당 메소드를 intrinsic 최적화 후보로 참고할 수 있다는 표시이며, 실제 적용 여부는 JVM 구현, 버전, 실행 옵션, CPU 지원 여부에 따라 달라질 수 있습니다.

#### 📊 성능 비교 *Integer.bitCount*
실제로 얼마나 차이가 나는지 확인하기 위해, 2진수 비트 중 1의 개수를 세는 *bitCount()* 로직을 **순수 자바 코드**와 **내장 함수**로 각각 실행해 비교했습니다.


```java
// 1. 직접 구현 - 내부 로직을 그대로 가져와 연산
// 실행 시간: 223,739 ns/op
public int customBitCount(int i) {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    // 비트 연산 생략...
    return i & 0x3f;
}

// 2. 내장 함수 사용 (Intrinsic)
// 실행 시간: 20,516 ns/op
public int intrinsicBitCount(int i) {
    return Integer.bitCount(i);
}
```

👉 **결과:** 10만 회 수행 기준, 내장 함수를 사용했을 때 **약 11배 더 빠른 성능**을 보였습니다.



------



### 4. 컴파일러 최적화

같은 '절댓값 합계'를 세 가지 버전으로 구현해 보았습니다.

1. **if-else 버전**
2. **삼항 연산자 버전**
3. **메소드 분리 버전**

```java
// if-else 버전
public int absSumIfElse(int[] arr) {
    int sum = 0;

    for (int val : arr) {
        if (val > 0)
            sum += val;
        else
            sum += -val;
    }

    return sum;
}

// 삼항 연산자 버전
public int absSumWithTernary(int[] arr) {
    int sum = 0;

    for (int val : arr) {
        sum += (val > 0) ? val : -val;
    }

    return sum;
}

// 메소드 분리 버전
public int absSumWithExtractedAbs(int[] arr) {
    int sum = 0;

    for (int val : arr) {
        sum += getAbsValue(val);
    }

    return sum;
}

private int getAbsValue(int val) {
    if (val > 0)
        return val;

    return -val;
}
```

| 버전 | 평균 |
| --- | ---: |
| if-else 버전 | `59.297 ms/op` |
| 삼항 연산자 버전 | `4.184 ms/op` |
| 메소드 분리 버전 | `4.174 ms/op` |

메소드 분리 버전은 if-else 로직을 `getAbsValue()`로 옮겼을 뿐인데, 왜 삼항 연산자 버전과 성능이 거의 같을까요?

#### 메소드 인라이닝(Method Inlining)

작은 메소드는 JIT에 의해 호출 지점에 본문이 직접 합쳐질 수 있습니다. 즉 자바 코드에는 `getAbsValue()` 호출이 보이지만, 실제 기계어에서는 `call` 없이 실행될 수 있습니다.

```java
sum += getAbsValue(val);
```

```java
// JIT가 인라이닝하면, 실행 시에는 본문이 들어간 것처럼 볼 수 있습니다.
if (val > 0)
    sum += val;
else
    sum += -val;
```

그래서 메소드 분리 버전은 호출 비용이 크게 드러나지 않았고, 삼항 연산자 버전과 비슷한 성능을 보일 수 있었습니다.

이번에는 JVM 옵션으로 `getAbsValue()`가 인라인되지 않도록 막고 다시 측정했습니다.

| 버전 | 평균 |
| --- | ---: |
| if-else 버전 | `58.356 ms/op` |
| 삼항 연산자 버전 | `4.359 ms/op` |
| 메소드 분리 버전 | `27.364 ms/op` |

인라이닝을 막자 메소드 분리 버전의 성능이 떨어졌습니다. 첫 번째 측정에서 삼항 연산자 버전과 비슷했던 이유 중 하나가 메소드 인라이닝이었음을 확인할 수 있습니다.

그런데 메소드 분리 버전은 여전히 if-else 버전보다 빠릅니다. 같은 `if-else` 로직인데, 왜 이런 차이가 남아 있을까요?

#### 비트 연산(Bitwise Operation)

자바 코드에는 `if-else`가 보이지만, JIT는 이를 분기 대신 `sar`, `xor`, `sub` 같은 명령 조합으로 바꿀 수 있습니다.

핵심은 실행 경로를 나누지 않고 같은 흐름 안에서 값을 계산한다는 점입니다. 다만 이런 최적화는 JVM 옵션, JIT 상태, CPU에 따라 달라질 수 있습니다.

### 마치며

이번 글에서는 정렬, Lookup Table, Branchless 코드, 내장 함수, JIT 최적화까지 몇 가지 예제를 살펴보았습니다.

처음에는 부등호 하나가 성능에 영향을 줄 수 있다는 말에서 시작한 궁금증이었지만, 실제 결과는 눈에 보이는 코드만으로 설명하기 어려웠습니다.

데이터의 패턴, JVM의 최적화, CPU가 실행하는 명령어까지 얽히면서 같은 자바 코드도 꽤 다른 성능 차이를 만들어낼 수 있었습니다.
