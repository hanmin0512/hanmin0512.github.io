---
layout: single
title: "AWS RDS 구성 실습"
categories: Cloud
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## AWS RDS 구성 실습
AWS RDS 구성 해보기

## 구성도
> ![스크린샷 2024-01-14 오전 2 15 35](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/63b3091a-8863-4764-839e-a06b9dea1e9c)

## VPC 설정 편집
> ![스크린샷 2024-01-14 오전 2 32 05](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/70bc223c-ea4f-4f12-b0a9-6965c0040924)
- VPC의 모든 부분에 IGW를 추가해야 한다. or NAT GW

## Create RDS
- MySQL 데이터베이스 생성 (version MySQL 8.0.34)
> ![스크린샷 2024-01-14 오전 2 20 47](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/8b60788f-5de6-46e6-b284-c78e88784a77)
> ![스크린샷 2024-01-14 오전 2 20 59](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/12f4a5a4-1043-4ff4-a020-5a8e1533c2c5)
> ![스크린샷 2024-01-14 오전 2 21 10](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/ffc92038-6be1-4ae6-ba7d-8e59dfe627d9)
> ![스크린샷 2024-01-14 오전 2 22 25](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/968e3ea3-2522-4cd0-9f4a-8fd1b22c5869)
> ![스크린샷 2024-01-14 오전 2 22 43](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/774b5b6d-0215-48ba-a50f-4ef8ccf7a194)
> ![스크린샷 2024-01-14 오전 2 23 53](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/a6bb2523-c4e2-4348-9816-1b19c330d144)
> ![스크린샷 2024-01-14 오전 2 24 29](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/fb38002b-0e43-414c-9145-d16b29dadd34)
- workbench로 연결하여 작업하기위해 퍼블릭 엑세스 활성화
> ![스크린샷 2024-01-14 오전 2 25 52](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/cc4be431-d9e6-496f-bc7c-c26144898d8e)
> ![스크린샷 2024-01-14 오전 2 29 48](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/d92be073-190d-452d-88a8-de84cb68e023)
> ![스크린샷 2024-01-14 오전 2 33 58](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/a30ba662-b004-4f44-b5b9-4b58c3564f7e)

> ![스크린샷 2024-01-14 오전 2 30 36](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/40bea8f0-773e-427c-94fe-1ff97d61afd7)

## Connect RDS
> ![스크린샷 2024-01-14 오전 2 42 32](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/e6888c94-8a19-4110-bda6-9c67af15a5ea)
- hostname
> ![스크린샷 2024-01-14 오전 2 44 00](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/a73684e0-187e-4c90-8cf0-9d38ab31e342)

> ![스크린샷 2024-01-14 오전 2 43 37](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/104990bc-1aca-4ca3-860b-0abe1bca51ed)
> ![스크린샷 2024-01-14 오전 2 45 47](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/43e3778a-7c74-4172-831b-67bd94a89bff)

```
create database db001;
use db001;

create table hanmin(
	empno INT NOT NULL PRIMARY KEY,
    ename VARCHAR(10));


insert into hanmin values (1, "hanmin");
select * from hanmin;
```
> ![스크린샷 2024-01-14 오전 3 17 37](https://github.com/hanmin0512/aws_RDS_Guide/assets/37041208/0b53a931-6c64-428c-8ec5-46677879ffdf)




 - <a href= "https://github.com/hanmin0512/aws_RDS_Guide"> AWS S3 가이드 github 보러가기</a>
