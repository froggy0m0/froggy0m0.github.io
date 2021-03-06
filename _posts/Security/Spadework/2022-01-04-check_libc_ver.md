---
layout: post
title: ✔삽질 노트✔ 맞왜안? libc 버전 확인
categories: [Pwnable, Spadework]
message: <span style="font-size:30px">맞</span>는데 <span style="font-size:30px">왜</span> <span style="font-size:30px">안</span>돼? 삽질 정리!

---

## libc 버전확인

> 로컬에서는 실행되고 원격에서는 안되는 경우.. 프로세스와 glibc 버전이 서로 상이한지 체크해보자!

문제에서 제공되는 라이브러리 파일의 `ELF`를 사용하여 주소를 구해야 된다.

<br>

다음은 서로 다른 버전(`libc.so.6`  , `libc-2.27.so`)의 puts 의 두 offset을 나타낸다.

```python
from pwn import * 
def slog(name, addr) : return success(" :".join([name.ljust(10), hex(addr)]))

lib_1 = ELF("./libc.so.6")
lib_2 = ELF("/lib/x86_64-linux-gnu/libc-2.27.so")

slog("libc.so.6_system_symbols", lib_1.symbols['system'])
slog("libc-2.27_system_symbols", lib_2.symbols['system'])

slog("libc.so.6_read_symbols", lib_1.symbols['read'])
slog("libc-2.27_read_symbols", lib_2.symbols['read'])
```

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-04-check_libc_ver-1.png">
    <figcaption>Fig 1. 두 라이브러리의 symbols 비교</figcaption>
</figure>


 리모트에서는  `libc.so.6` 이 사용되었다고 해도 로컬에서는 적용된 라이브러리 (Ubuntu 18.04는 `libc-2.27.so`를 ) 사용 하기 때문에  `libc-2.27.so`를 사용하여 구한 주소를 사용해서 잘 되겠지만 리모트에서는 `libc.so.6`을 통해서 구해야한다.

### 예제) 그래서 libc가 다른경우

만약 원격이 libc-2.27.so 의 환경이지만, 로컬에서 libc.so.6로 계산하여 작성한 경우를 예로 들겠다.

> leak 된 read함수를 예로 libc_base 주소와 system함수의 주소를 계산
>
> libc_base_addr =  read address + libc.so.6_read_symbols(<span style="color:lightgreen">0xf7250)</span>
>
> system_addr = libc_base_addr + libc.so.6_system_symbols(<span style="color:skyblue">0x110140)</span>

 libc.so.6를통해 read함수의 주소를 얻어내고 ret주소에 system함수를 넣은 경우

<figure>
    <img src="https://Froggy0m0.github.io/assets/img/Spadework/2022-01-04-check_libc_ver-2.png">
    <figcaption>Fig 2. ret에 잘못 계산된 주소 예</figcaption>
</figure>

위의 gdb 사진처럼 <span style="color:#4FFF00; background: linear-gradient(to top, #FF0000 10%, transparent 25%)"><__libc_csu_init+100></span>에서 <span style="border:1px solid; border-color:red">0x7f6be2c9b280</span> 로 ret하게된다.

분기된 주소( <span style="border:1px solid; border-color:red">0x7f6be2c9b280</span> )에는 계산된 system함수의 주소가 들어가야 하지만 의도와는 다르게 __vfprintf의 +11760__ 주소이다.

<br>

그럼 제대로 계산을 다시하자면

>libc_base_addr  = 0x7f6be2c9b280 - libc.so.6_read_symbols + libc.so.6_read_symbols - libc-2.27_read_symbols
>
>system_addr = libc_base_addr + libc2.27_system_symbols
>
><br>
>
>> 즉,
>>
>>  <span style="color:purple">libc_base_addr</span> = <span style="border:1px solid; border-color:red">0x7f6be2c9b280</span> - <span style="color:#FFD400">0x45390</span> + <span style="color:lightgreen">0xf7250</span> - <span style="color:skyblue">0x110140</span>
>>
>>  <span style="color:orange">system_addr</span>= <span style="color:purple">0x7f6be2c3d000</span> +<span style="color:brown">0x4f550</span>
>



