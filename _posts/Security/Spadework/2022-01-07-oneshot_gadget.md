---
layout: post
title: ✔삽질 노트✔  oneshot
categories: [Pwnable, Spadework]
message: one_gadget 삽질 정리!
---

## one_shot

__one_gadget__ 처음 사용해보고  __one_gadget__ 을 의도한 문제는 아니었지만 __one_gadget__ 으로 풀었다..

푸는 과정에서 모든 __one_gadget__ 이 모든 프로세스에 실행되지 않다는 것을 알았다.

### process

```c
long * input;
input = (long *)malloc(1024);
read(0, input, 1024);
*(long *)*input = *(input+1);
free()
```

위와 같이 사용자 입력 주소 값에 값을 덮어쓸 수 있는 프로세스가 있다.

<br>

### payload 작성

다음 과 같이 `__free_hook 주소` + `분기를 원하는 주소`로 payload를 작성하면 

free함수를 실행하는 라인에서 원하는 주소로 분기 할 수 있다.

```python
payload = p64(__free_hook_addr) + p64(one_gadget_addr)
p.send(payload)
```

### oneshot gadget 얻기

one_gadget 을 사용해 얻은 oneshot gadget의 offset이다.

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-oneshot_gadget-1.png">
    <figcaption>Fig 1. Onegadget Offset</figcaption>
</figure>


이중 __0x4f432__ 를 사용하였다.

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-oneshot_gadget-2.png">
    <figcaption>Fig 2. Get_Shell</figcaption>
</figure>


### <span style="color:skyblue; background: linear-gradient(to top, red 10%, transparent 15%)"> 0x4f3d5</span> (좌측)   와 <span style="color:lightgreen; background: linear-gradient(to top, orange 10%, transparent 15%)"> 0x4f432</span>(우축) 비교

다른 원샷 가젯들(__0x4f3d5__ , __0x10a41c__)로 payload를 작성해봤는데 쉘을 얻어낼 수 없었다.

약간의 삽질 에 확인 할 수 있었던 것이 있었는데.

위 Fig 1. Onegadget Offset 에서 표시한 것처럼

> __0x4f3d5__
>
> <span style="background: linear-gradient(to top, blue 10%, transparent 15%)"><span style="color: #4FFF00">rsp</span> & 0xf == 0<br><span style="color: #4FFF00">rcx</span> == NULL</span> 




> __0x4f432__
>
> <span style="background: linear-gradient(to top, blue 10%, transparent 15%)">[<span style="color: #4FFF00">rsp</span>+0x70] == NULL</span> 

를 확인해 볼 수 있다.

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-oneshot_gadget-3.png">
    <figcaption>Fig 3. 두 원샷가젯(<span style="color:skyblue; background: linear-gradient(to top, red 10%, transparent 15%)"> 0x4f3d5</span>과 <span style="color:lightgreen; background: linear-gradient(to top, orange 10%, transparent 15%)"> 0x4f432</span>)의 레지스터 값 </figcaption>
</figure>


<span style="color:skyblue; background: linear-gradient(to top, red 10%, transparent 15%)"> 0x4f3d5</span>(좌측)에서는 $rsp = <span style="background: linear-gradient(to top, red 10%, transparent 15%)">0xf508cd17</span> $rcx =  <span style="background: linear-gradient(to top, red 10%, transparent 15%)">0xf0003d48</span> 로 

> <span style="background: linear-gradient(to top, blue 10%, transparent 15%)"><span style="color: #4FFF00">rsp</span> & 0xf == 0<br><span style="color: #4FFF00">rcx</span> == NULL</span> 
>
> 를 만족하지 못하고 <do_system+1094> 에서 실행이 끝난다.

<hr>
<span style="color:lightgreen; background: linear-gradient(to top, orange 10%, transparent 15%)"> 0x4f3d5</span>(우측)에서는 $rsp = <span style="background: linear-gradient(to top, orange 10%, transparent 15%)">0xf508cd17</span> $rcx =  <span style="background: linear-gradient(to top, orange 10%, transparent 15%)">0xf0003d48</span> 로 

> > <span style="background: linear-gradient(to top, blue 10%, transparent 15%)">[<span style="color: #4FFF00">rsp</span>+0x70] == NULL</span> 
> >
> > 를 만족하며 /bin/sh를 얻어낼 수 있었다.

<hr>
<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-oneshot_gadget-4.png">
    <figcaption>Fig 4. 레지스터값 변경전</figcaption>
</figure>

one_gadget<span style="color:skyblue; background: linear-gradient(to top, red 10%, transparent 15%)"> 0x4f3d5</span>을 사용하여 RCX와 RSP를 다시확인해보았다.

> RCX = 0x7f3c7ea99151 (read+17)
>
> RSP = 0x7ffde5687518 -> 0x7f3c7ea20d17

<hr>
<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-oneshot_gadget-5.png">
    <figcaption>Fig 5. 레지스터값 변경후</figcaption>
</figure>


이후   gdb에 *rsp = 0, rcx = 0을 넣어 만족하도록 설정해봤다.

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-07-spadework2-6.png">
    <figcaption>Fig 6. 쉘 획득!</figcaption>
</figure>
변경 이후 계속 실행하여<do_system+1180> 까지 실행되고 쉘을 얻어낼 수 있었다.

> __추가__
>
> > 포스팅 수정을 위해 같은 오프셋을 가진 원샷가젯으로 실행했을 때는 정상 실행 되는 것을 확인했다. 원샷 가젯을 이용할 때 레지스터 값 에 따라 오류가 발생할 수도 있고 상황에 맞게(입력 길이의 여유) payload를 보내서 레지스터 수정하여 실행할 수 도 있을 것이다.

#### ??......<do_system+1094>?

이전 rop 에서도 `do_system+1094`에서  오류가 났었다 __movaps__ 함수는 16바이트가 아닌 데이터에서 작동하면 오류를 일으키는 것 같다.. 그래서 pop_ret 가젯위에서 8바이트를 추가해서  16바이트로 정렬하여 페이로드를 보냈던 기억이 있다.

본 문제는 보호 기법 PIE도 적용되지 않았고 원샷가젯을 이용하지 않고 `system("/bin/sh")` 를 ret주소로 넣으면 되지만 원샷가젯을 이용하였다가 이후에 ret에 `system("/bin/sh")`를 넣어보려고 시도하였다.



```assembly
#assembly는 이런내용
mov	edi, 0x400aeb	#0x400aeb == "/bin/sh"문자열
call 0x400788 <system@plt>
...
```

system함수 인자인 edi 에 문자열 '/bin/sh'를 집어넣고 __system함수주소(0x400788)__ 를 불러내면 쉘 획득!

.........근대 여기서 ret에 call함수 주소(__call 0x400788 \<system@plt>__)로 넣었더니 <do_system+1094>에서 멈춰버렸다.(인자 값이 없으니까!)

그래서 gdb에서 __/bin/sh(0x400aeb)__ 를 집어넣으면 되겠지 했는데 ... 안되네..

리모트에서 ret에 mov함수 주소부터 넣으면 잘되는데 로컬에서는 제대로 작동도 안되고 ...

