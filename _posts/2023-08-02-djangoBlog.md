---
layout: single
title: "django로 만든 웹 프로젝트"
categories: WebProject
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## 이 웹 프로젝트는 군인 시절에 django 책을 보고 만든 웹 사이트이다.
---
## 개발 환경
- python
- python의 웹 프레임워크 django 사용
- goormIDE (ubuntu)

## 시스템 아키텍쳐
- E-R Diagram ![E-R 다이어그램](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/a9ec1a2a-5894-4c70-9942-9bc24e459738)


## 실행 화면 
- WAS 실행 <br> ![0](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/b8c770b8-de6c-4b5a-9e62-8694a1d6f8fd)

- 처음 화면 [로그인/회원가입] ![1](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/c26b8552-00f7-4679-9832-df4e808900af)

- 회원가입 화면 ![2](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/e4bf4410-59b5-45fe-ab0c-32ee478f8c43)

- 회원가입 성공 화면 ![3](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/edfbe000-be54-4f9c-8608-9301205e6e19)

- 로그인 화면 ![4](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/27e27416-5862-4438-bea3-c4dd3c83b007)

- 로그인 후 앨범 추가 선택 ![5](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/6a84b3e0-d937-4c8f-9458-4f0564116387)

- 앨범에 사진 및 설명 기입 화면 ![6](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/2f70da00-628d-44d9-80cc-c8ebe996f049)

- 앨범 생성 완료후 앨범 사진들이 시간차 공개 
![7-1](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/4bd46366-279c-4116-bee9-d8c08e01fd4a)
![7-2](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/28157a6f-c0d5-4024-8158-bfa5d96953a3)

- 앨범 클릭시 게시물화 된 화면 ![8](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/2e925c48-d4b2-48ec-bfce-284a7db0190b)

- 앨범에 새로은 사진 추가 하기
![8-1](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/6dd8fbe8-a9ed-4e05-91d7-9f9e33f921d8)

- 추가완료
![8-2](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/55397718-1494-4825-9e2b-869385073b8f)


- 상단 게시물 탭 클릭시 다른 사람이 올린 게시물도 확인가능 [인스타그램 참조]
![9](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/f4fe24d8-83a7-418f-ae3d-11a4f0e354e3)

- 게시물 검색 기능 게시물 제목으로 검색 가능
![10](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/deff8b4b-105c-46f4-9554-c71a7cf3f2cf)

- 검색된 게시물 클릭시 큰 화면으로 게시물 포커싱(댓글기능 가능 외부 모듈)
![11](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/ada2ba71-1b4f-4ed0-8978-fd4ddc48e9f1)

- 게시물 올리는 화면
![12](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/86eedebd-0595-48e2-b2e5-39a4c05dd8aa)

- 게시물 올라간 화면
![13](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/24960c52-eec1-43b5-9545-750e25fc4329)


- 게시물 소유자는 게시물 삭제 가능
![14](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/09757e1a-ccf8-44c4-b506-f9b460958c77)

- 전체 클라이언트의 데이터를 관리하는 페이지 (관리자 로그인 화면)
![15](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/0b436751-ad09-44ad-a593-c80ff69b6b7c)

- 관리자 페이지 화면 모든 데이터 권환 소유
![16](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/3ece0b5f-ee3e-4cf6-a9ea-87095223a426)

- root가 클라이언트 user1의 앨범 삭제
![17](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/4864f53f-71f8-492e-a019-3e0cd7a37dbc)

- 앨범 삭제 2차 확인
![18](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/1f37d8f9-987a-4b7d-8343-9977ebf45ed2)

- 삭제 완료 메시지
![19](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/225f6e28-2e66-4b1b-a928-03dea32009ff)

- 게시물 삭제 화면
![20](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/6bb5b56a-6528-40de-99ef-bdeb292c5360)

- 게시물 삭제 2차 확인
![21](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/b30344d9-3dcb-4e32-bdac-4c8e7b3206c2)


- 삭제 완료 메시지 및 게시물 목록 테이블
![22](https://github.com/hanmin0512/soldier_django_blog/assets/37041208/4b9eacdb-9308-441a-a184-3b8b8d5ba750)


## 코드 보러가기 (github)
- <a href= "https://github.com/hanmin0512/soldier_django_blog"> django web 앱 코드(github)</a>