---
layout: single
title: "AWS EndPoint 구성 실습"
categories: Cloud
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## AWS VPC Endpoint 실습
- aws vpc endpoint 실습
- aws s3 서비스와 private하게 연결하기 위해서
- VPC privatesubnet 부분에 ec2와 앤드포인트 설정을 한다.
- private 노드에 접근을하려면 먼저 같은 VPC에 public 노드로 접근을 한후
- public 노드에서 private노드로 접근하는 방법을 선택한다.
- 그 후 private 노드에서 앤드포인트로 S3와 private하게 연결하여 서비스를 이용한다.

## 네트워크 구성도
> <img width="609" alt="스크린샷 2024-01-14 오전 2 13 04" src="https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/502111b1-4d9e-4dd2-811d-03809d26d9f9">

## Create EC2
- BastionServer(EC2 -Public VPC) 생성
> ![스크린샷 2024-01-14 오전 12 55 28](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/6d06e2ba-87b2-47e2-a5c6-68d45f0f11ea)
> ![스크린샷 2024-01-14 오전 12 55 47](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/d39461f7-0959-4dbc-8adc-cb85fed0a935)
> ![스크린샷 2024-01-14 오전 12 56 21](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/869d4122-2fb7-4bdd-8154-be435ae9cb45)
> ![스크린샷 2024-01-14 오전 12 56 35](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/d0396ca7-2112-4566-8ce5-0498581db881)

- EndPointServer(EC2 -Private VPC) 생성
> ![스크린샷 2024-01-14 오전 12 58 10](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/bfc24cfc-f6b7-4f6c-9734-ff167794ae8a)
> ![스크린샷 2024-01-14 오전 12 58 19](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/77c72e18-3c49-4773-bc47-3756e3d9c222)
> ![스크린샷 2024-01-14 오전 12 58 40](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/6a5a9bfd-40e1-4ecf-b171-8f63c20e208f)
> ![스크린샷 2024-01-14 오전 12 58 52](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/c29af013-680c-4c88-a04c-4de2e8820786)

## Create Bucket
- S3 Bucket 생성
> ![스크린샷 2024-01-14 오전 1 01 16](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/5bedff6a-a3dc-40e2-9761-b0bd1c11e37b)

- 보안상 옳진 않지만 bucket에 키페어를 업로드 해준다. (원래는 ssh 키 포워딩을 사용해야 안전하다.)
- 보안상 키페어 업로드 이미지 생략

## BastionServer Connect
- BastionServer에 ssh로 접근한다.
- IAM에서 발급 받음 public key, private key를 입력한다.
- CLI로 S3에 연결하여 EndpointServer에 연결할 키를 다운받는다.

> ![스크린샷 2024-01-14 오전 1 15 30](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/2d03b00f-7316-497b-a852-2148c395b60c)
> ![스크린샷 2024-01-14 오전 1 25 37](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/045dca41-3f34-4720-826a-13e705690cb6)

```
aws s3 ls s3://[Your_Bucket_Name]/
aws s3 ls s3://[Your_Bucket_Name]/[Object_Name] .
```
> ![스크린샷 2024-01-14 오전 1 29 59](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/e2a3081b-42e7-494d-a0c9-07eed2ac51c2)

## EndpointServer Connect
- 다운로드 키페어를 통해 ssh로 EndpointServer에 연결해준다.
> ![스크린샷 2024-01-14 오전 1 42 36](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/a7afb473-8e8d-4ccb-8329-f39672625c35)
> ![스크린샷 2024-01-14 오전 1 43 21](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/84f2c80f-bb57-4eb9-baed-4f59ca9ac4fc)
- endpoint가 설정이 안되어있어서 aws s3 ls가 안되는 모습 
> ![스크린샷 2024-01-14 오전 1 44 50](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/be553054-7ba6-4541-a2c9-3fa2e3caf96a) 

## Create EndPoint
- 앤드포인트 생성
> ![스크린샷 2024-01-14 오전 1 47 35](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/f0b2f331-629e-44fc-895a-18e23ec93638)
> ![스크린샷 2024-01-14 오전 1 48 13](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/000ba1a1-1dc4-4c3d-b57e-416d3e22b583)
> ![스크린샷 2024-01-14 오전 1 48 55](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/90ca1c21-2ef8-4155-ad93-6521c6daf38a)
> ![스크린샷 2024-01-14 오전 1 52 17](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/72a7f86a-fec1-4148-a522-8104ca99ce55)

## EndpointServer - S3 Bucket EndPoint Connecting
- private 서버인 EndpointServer와 Bucket을 앤드포인트로 연결하기.
> ![스크린샷 2024-01-14 오전 1 53 19](https://github.com/hanmin0512/aws_VPC_Endpoint_Guide/assets/37041208/56849b54-f74a-4bf9-b58a-76ed5e00575b)

- <a href= "https://github.com/hanmin0512/aws_VPC_Endpoint_Guide"> AWS End Point 실습 github 보러가기</a>



