---
layout: single
title: "취약한 쇼핑몰 웹 서비스 Hacking 실습"
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## Web-hacking-LAB
취약한 사이트에 웹해킹을 해보자!
- 취약한 사이트 소스를 서버에 올려 준비하기.
## 정보 수집
1. 웹페이지 살펴보기.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/8d0f600d-4766-4a66-8785-4e0bd7385673)
- nmap을 통해 사이트의 정보를 파악하기.
> ![nmap](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/226cee5c-ec94-4ab2-818d-ec716b1c9b8e)
- 웹서버는 Microsoft IIS httpd 6.0을 통해 호스팅 하고 있다.

2. nikto를 통한 정보 수집.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/c35870d6-542d-4b53-9ecf-1d0753c50a12)
- CVE-2000-0649 취약점이 발견 되었다.

## 계정정보 추측 및 대입
<table>
  <tr>
    <th>분류</th>
    <th>코드</th>
    <th>점검 항목</th>
  </tr>
  <tr>
    <td rowspan="2">계정정보<br>추측 및 대입</td>
    <td>BP-001</td>
    <td>취약한 패스워드 설정 여부</td>
  </tr>
  <tr>
    <td>BP-002</td>
    <td>어플리케이션/장비 기본 패스워드 설정 여부</td>
  </tr>
</table>

1. BP-001: 취약한 패스워드 설정 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/46e316de-101a-4cc1-b9b1-f1241ecd2bd2)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/25bd9f18-27de-45bc-a0b0-5124540c59e6)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/6b225ef2-d98d-4abe-a904-0d56869fc769)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/3207b729-1f54-4d47-aa07-80332308afb1)
2. BP-002: 어플리케이션/장비 기본 패스워드 설정 여부 [취약한 패스워드 회원가입 시도]
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/537d503c-071a-4db2-9643-c4089e135969)

## XSS 취약점
1. Q&A 게시판 내용 쿠키 및 주요 정보 탈취 bXSS 삽입
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/2dabd1fc-672e-4306-9482-5d8475df0b26)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/512b8586-a04e-4103-967a-53683017c172)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/b1a8e519-90a0-4d23-88c9-aa14ceb1c97e)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/f0fa4d2c-fc87-497c-95db-edd2c6d8a6ed)
2. Bxss 헌터 사이트로넘어가 쿠키 데이터를 탈취.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/9eb1758c-1cf8-476e-9a2f-e7f581ffc02b)
3. 쿠키정보를 재사용 하여 접근한 사람의 쿠키를 재사용하여 계정 로그인.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/52833c65-8240-4319-8c16-f8d7887ca720)
4. 탈취한 쿠키로 로그인한 개인정보 확인.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/e2cc4523-8450-4f2a-bc63-abe08c0a8e2c)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/6b787f1d-3d2e-43a7-a525-a4e88e511783)

> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/169a031a-8518-40d6-be01-03f97676cabd)
- 위의 XSS모의 해킹 과정을 통해 Q&A게시판의 XSS취약점을 이용하여 bxss-Hunter웹을 사용하여 악의적인 스크립트를 삽입하여 게시물의 접근한 사람의 계정 정보를 탈취 할 수 있는 과정에서 쿠키 재사용 여부를 확인 할 수 있엇고, 비밀번호를 몰라도 비밀번호 변경이 가능한 취약점을 발견했다.

## 에러 메시지 처리
1. 에러 메시지를 통한 중요/불필요한 정보 유출
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/a67513eb-cc49-40d7-9282-b501afb83285)
- 불필요한 정보 유출을 통해 DB Server정보 및 SQL Injection 공격 여부 확인이 가능하며, 특정 소스코드파일의 특정 라인의 에러 유형 정보를 유출하여 접근 가능성 여부를 확인 할 수 있음.

## 관리자 페이지 추측
1. 페이지 내 관리자 페이지 링크 여부[있음]
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/bb844291-4854-4422-9d9b-17c42b1936ea)
2. 관리자 페이지 접근 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/c2ec6989-dad3-44b0-a575-07f6b0e974e4)
- 위 과정을 통해 누구나 관리자 페이지에 접근이 가능하며 관리자 계정 탈취를 당했을 시 웹사이트의 모든 권한을 갖는 취약점을 발견.

## 페이지내 중요 정보 노출
1. 중요 개인정보 노출 여부[있음]
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/0032f9bb-4bd5-4441-8771-f205ee8e471e)
- Q&A 게시물에 접근하여 크롬 개발자 도구 [F12] -> [Element] 페이지 소스에 게시물 작성자 이메일 노출되어 게시물 작성자에게 피싱 이메일, 스팸 및 불법광고, 사회공학적기법등의 해킹 공격이 시행 될 가능성을 발견.

2. 쿠키 값 내 중요정보 유출 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/eb68128e-9de3-46f1-a6ad-dfbf21d4e627)
- 개발자도구를 -> [Application] ->[Cookies] 탭에 보면 oyes_order 주문과 관련된 정보를 담고있는 쿠기가 유출 되었으며, 세션 관리에 사용되는 ASPSESSIONID는 해당 세션에 연결된 사용자를 식별. 세션 ID가 노출되면 세션 관리 기능이 우회되어 보안 위협이 발생. MemLog 쿠키는 어떤 종류의 로깅 또는 메모리 로그인지를 확인 가능.
3. 데이터베이스 관련 정보 유출 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/cbd9eae1-bb85-48f8-8e1c-089a7ac3bcdc)
- Q&A 게시판 검색 Form에 싱글 쿼터(‘)를 입력하면 Database Server 정보와 서버 단에서 DB Server로 전송하려는 SQL query문을 추측 가능.

4. 중요정보 평문전송
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/8e1faaa3-1874-4be3-a72f-7d849fa7cc52)
- 사용자가 로그인을 시도 하였을 때 로그인 데이터를 평문으로 전송. 악의적인 사용자와 같은 네트워크를 사용 했을 때 sniffing 공격으로 로그인 데이터를 탈취당하는 취약점을 발견.

## 파일 다운로드 취약점
1. 입력 값 검증 미흡으로 파일 다운로드 공격 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/a76b68cf-0cbd-4813-8d2d-2a5eafd6092f)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/3d58c702-12e5-4faa-962a-b3e1882fed48)
- burpsuite를 활용하여 파라미터 입력값 변조하여, admin_login_ok.asp 소스파일 다운로드 완료.
- 입력 값 검증 미흡으로 인한 파라미터 입력값을 변조하여 admin 로그인 정보를 확인하는 소스파일을 다운로드가 되는 취약점을 발견.

## 파일 업로드 취약점.
- 입력 값 검증 미흡으로 파일 업로드 공격 여부.
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/f4e567dd-ed19-494e-8474-b1a53c9c5d8f)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/31b44f9b-12d7-4ebf-816e-ad461de1d9d3)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/aceb6c3c-bae3-4584-92e3-a7f4227b72c9)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/229e3d01-083f-44e0-9854-7ec4a9a7aa0f)
- 입력 값 검증 미흡으로 인해 파일의 확장자명을 proxy를 거칠 때 변경하여 서버에 파일이 업로드 된 것을 볼 수 있음, 위 파일은 webshell파일이므로 웹에 대한 파일 권한을 획득하여 악성코드 파일 업로드 취약점을 발견 했다.

## SQL Injection
1. SQL Injection 허용 여부
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/175ef921-1e3f-4392-93a6-24690ad50f51)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/b458dc15-a354-4c16-821a-e6704406af6e)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/4c4c83d6-de84-45a5-bf52-11e6c454389a)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/3cb19a3c-2d30-4384-9115-beea071abba2)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/102a2658-5023-4062-a85b-3c75933d0cda)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/220ce9e7-a6ca-4f37-82fc-80d4f47e13a7)
- SQL injection을 허용하는 Form을 발견하여 union 공격을 시도하여 관리자 test 계정과 관리자 페이지 계정 정보를 조회 할 수 있는 SQL injection 취약점을 발견했다. 이 취약점은 웹 서버 운영에 큰 피해를 줄 수 있는 큰 취약점 발견.

## 부적절한 서버 설정
1. 서버 설정 여부[부적절]
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/23a07637-9918-48ef-8c7b-c2eed8d7c07e)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/f4de18b1-4500-4474-ab05-52219bb4af96)
- 웹서버 부적절한 설정으로 탈취한정보로 로그인을 했으나 오류 메시지 반환.
2. 파일 업로드 취약점을 이용하여 웹쉘을 통해 소스파일 분석
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/6ebde9e8-64ae-4589-852b-c312ee0759b0)

3. sqlmap을 사용하여 sell_man 테이블 dump
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/505e9dfa-5e23-4a4a-9ae6-1ba0ab372ffc)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/04f2330d-da85-474b-8bf3-d2a02bdf8d43)
> ![image](https://github.com/hanmin0512/Web-hacking-LAB/assets/37041208/f6e4dbe6-024d-4ce0-8541-54df9fb0f8de)
- 부적절한 웹 서버 설정으로 인한 오류를 반환 하는 페이지 정보를 수집하였고, 파일 업로드 취약점을 이용하여 웹쉘을 업로드하였고 이를 이용하여 수집한 정보를 기반으로 소스코드를 모두 확인하여 oyesmall데이터베이스의 sell_man 테이블의 total_p컬럼이 <blank>여서 올바른 데이터임에도 접속이 불가능 하였다. 또한 SQL Injection이 통하는 폼을 위에서 찾았기에 union문을 통해 sell_man 테이블의 total_p 컬럼의 값을 넣어 변조하였고, 다시 admin페이지에 로그인 성공을하여 admin 권한을 탈취.




- <a href= "https://github.com/hanmin0512/Web-hacking-LAB"> 취약한 쇼핑몰 웹 해킹 github 보러가기</a>






