---
layout: single
title: "Dreamhack [Command-Injection-1] 문제풀이"
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 드림핵 문제풀이 [command-injection-1]

## 코드 분석
> 전체 코드
> ![0](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/ee5d6f22-344f-4f1c-bcc2-8c4c256f882c)

> 유심히 봐야 할 곳 1. app.py

```c

cmd = f'ping -c 3 "{host}"'

```
- 폼에 입력한 문자열이 {host} 이부분으로 들어간다. python 문법이다.

> 유심히 봐야 할 곳 2 ping.html

```c
 <input type="text" class="form-control" id="Host" placeholder="8.8.8.8" name="host" pattern="[A-Za-z0-9.]{5,20}" required>
```
-입력값이 영문 대/소문자, 숫자, 그리고 점(".")으로 이루어져야 한다는 뜻으로 공백이나 다른 특수문자는 입력하면 안된다는 뜻이다. 그래서 우린 burpsuite를 이용해 입력값을 조작할 것이다.


## burpsuite로 조작해보기.
> 올바른 데이터 전송해보기.
>  ![1](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/09f0ff78-fcad-4e3f-97c1-74f7d05719a9)
> 데이터 조작해보기 <br>
> ![2](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/9cb2a519-f63d-40b0-88c0-7d6f826b20b8)
> ![3](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/bf61a14c-e8bb-4db2-b811-d651e2e13639)
- ping -c 3 "8.8.8.8; cat flag.py"    -----> "{host}" 는 "8.8.8.8; cat flag.py" 가 되는 걸 알수 있다.

## 실제 파이썬에서 어떤 문자열 형태가 전송 될까?
> ![4](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/dab40e00-9d19-4f71-a2e3-86781d7c5bc1)

- ! 추가로 host = 8.8.8.8"; cat flag.py " 이렇게 조작해서 데이터를 전송해도 flag가 나오지만 리눅스 터미널에서는 "" 이부분 때문에 에러가 난다. 하지만 # 부분은 에러가 나지 않고 뒤에 명령어가 잘 실행 되므로 # 공격으로 해보겠다.


## burpsuite를 활용한 공격
> ![5](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/babb07cf-1064-426a-a7a5-9d8afd448e1d)

> ![6](https://github.com/hanmin0512/dream_hack_command_injection/assets/37041208/fadcd5ed-b231-47db-8f5c-42d2c8092e53)

- 문제 풀이 성공!