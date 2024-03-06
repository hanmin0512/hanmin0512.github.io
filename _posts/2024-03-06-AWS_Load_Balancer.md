---
layout: single
title: "AWS Load Balancer 구성 실습"
categories: Cloud
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## AWS LoadBalancer 구성 실습
aws 로드 벨런서의 ELB, ASG 인프라를 구성해보자.

## 대상 그룹(Target Group) 생성
- EC2 > 대상 그룹 > 대상 그룹 생성
- 대상 그룹 : ELB 뒷단에 있는 서버들의 집합을 의미
> ![스크린샷 2024-01-20 오후 11 21 57](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/d98e00da-50b9-4cd1-835e-a9339bb64a80)

> ![스크린샷 2024-01-20 오후 11 22 10](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/951d7665-c3aa-4c53-a3ac-839a84ec3f4a)

- 다음 > 대상 그룹 생성 클릭

## ELB 생성
- EC2 > 로드 벨런싱 > 로드 벨런서 > 로드 벨런서 생성 > Application Load Balancer 생성
> ![스크린샷 2024-01-20 오후 11 29 57](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/cc4a9100-74d1-4bf7-920f-6bd23b0ecf22)
> ![스크린샷 2024-01-20 오후 11 30 15](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/f98e86b1-16a9-451b-9856-eb62dac91c1e)
> ![스크린샷 2024-01-20 오후 11 31 43](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/aaa38b8a-5666-44f7-8a39-a83918d7ac56)


## 시작 템플릿 생성
- EC2 > 인스턴스 > 시작 템플릿 > 시작 템플릿 생성
> ![스크린샷 2024-01-20 오후 11 34 53](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/94fbcc7e-401e-4388-9e8c-948b9311a7e7)
> ![스크린샷 2024-01-20 오후 11 35 16](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/7af70760-9cbc-4782-a991-d6294c6127dd)
- 혹은 본인 AMI가 있다면 본인 AMI를 선택 해두 된다.
> ![스크린샷 2024-01-20 오후 11 35 49](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/3c901267-8664-42a3-8ae8-2756080cb442)

## Auto Scaling Group 생성
- EC2 > Auto Scaling > Auto Scaling 그룹 > Auto Scaling 그룹 생성
> ![스크린샷 2024-01-20 오후 11 37 56](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/1291c9e0-a21a-4996-9a0f-6fb1065e98b8)
- 앞 단에 ALB가 있기 때문에 private 서브넷에 오토스케일링을 둔다.
> ![스크린샷 2024-01-20 오후 11 39 02](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/d5c02b76-533b-4881-a592-13debbb458df)
> ![스크린샷 2024-01-20 오후 11 39 46](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/7128a27b-f241-4d15-a095-f2f4a37c51f1)
- Auto Scaling + ELB 설정 체크
> ![스크린샷 2024-01-20 오후 11 40 24](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/358893e1-2d51-46d9-8586-3280dcde0d1a)
> ![스크린샷 2024-01-20 오후 11 41 25](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/349974d7-10f2-4916-91c5-12b3c8fe5fb6)

- 다음 > 다음 > 다음 > Auto Scaling 생성
- EC2 생성 된것을 확인 가능.
> ![스크린샷 2024-01-20 오후 11 43 17](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/53698acf-669a-4217-855f-f64c5cbf3a91)

## EC2 
- EC2하나를 종료 하면 하나가 더 생성 되는지 확인.
> ![스크린샷 2024-01-20 오후 11 44 53](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/03d9eb5c-8504-4840-b2eb-a1aefa5ddc40)

- 자동으로 하나가 더 생성되는 것을 볼 수 있음.
>![스크린샷 2024-01-20 오후 11 47 17](https://github.com/hanmin0512/aws_LoadBalancer_Buide/assets/37041208/150686b4-71b1-407b-940b-1453a56a6b90)


 - <a href= "https://github.com/hanmin0512/aws_LoadBalancer_Guide"> AWS Load Balancer 구성 실습 github 보러가기</a>