---
layout: single
title: "Dreamhack [simple-sqli] 문제풀이"
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 드림핵 문제풀이 [simple-sqli]

## 코드 분석
- 전채 코드
> ![0](https://github.com/hanmin0512/dreamhack_simple_sqli/assets/37041208/0f7b1dea-c965-497b-847f-e9cfc2a4ceff)
> ![1](https://github.com/hanmin0512/dreamhack_simple_sqli/assets/37041208/cf3b409e-3424-4c79-b8f1-679ec126eb7e)
- 유심히 봐야 할 곳 app.py
```c
res = query_db(f'select * from users where userid="{userid}" and userpassword="{userpassword}"')
if res:
    userid = res[0]
    if userid == 'admin':
        return f'hello {userid} flag is {FLAG}'
    return f'<script>alert("hello {userid}");history.go(-1);</script>'
return '<script>alert("wrong");history.go(-1);</script>'
```
- 이 부분은 쿼리의 결과 값이 오류없이 잘 왔다면 if문 안으로 들어간다.
- 결과 값이 저장된 리스트에 0번 인덱스 값이 admin이라면 flag를 아니라면 alert를 실행 하는 코드이다.

## SQL Query 조작하기.
- 서버를 실행하고 login버튼을 눌러보면 로그인 폼이 나온다.
- ID 입력 폼 값이 {userid} 부분이 된다.
- PW 입력 폼 값이 {userpassword} 부분이 된다.
- 플랫폼마다 다르지만 sql 문법에서 대부분 주석은 "--" 이다.
- select * from users where userid="{userid}" 이 부분을 제외한 부분은 날려 버릴 것이다.
- ID 입력 폼에 admin" -- 를입력 한다. PW 부분은 주석처리 될 곳이므로 아무거나 입력한다.
- f'select * from users where userid="admin" --[" and userpassword="{userpassword}"'] 주석 처리 됨.

## 실습
> ![2](https://github.com/hanmin0512/dreamhack_simple_sqli/assets/37041208/24dde3a5-0094-42be-b994-1556d8ca3693)

- 성공!
> ![3](https://github.com/hanmin0512/dreamhack_simple_sqli/assets/37041208/84516de8-b3b0-489a-91b3-2dbbca65268a)