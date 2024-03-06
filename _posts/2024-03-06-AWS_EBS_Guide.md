---
layout: single
title: "AWS EBS 구성 실습"
categories: Cloud
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## aws_EBS_Guide
aws Ec2에 새로운 EBS를 연결하는 실습

## EBS 개념
- Elastic Block Store로 스토리지 서비스이며 ec2에 연결하여 사용할수 있는 ssd,hdd 같은 개념이며, 용량이 부족하면 유연하게 추가 할 수 있다.
- 지금은 용량을 추가하는 외장 하드 같은를 추가하는 듯한 실습을 EBS볼륨을 이용하여 해보겠다.
## 사전 준비
- EC2 생성 (VPC, 퍼블릭 IP 자동 할당 : 활성화)
- VPC 구성

## 실습
- 볼륨 생성
> ![1](https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/7ec41d49-0ac8-4553-ae4d-25a3fb52e05f)
- 생성 완료
> ![2](https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/b8006732-399f-48b3-bc6c-935fe46f0a0e)
- 볼륨을 EC2에 연결 (디바이스이름 dev/sdf 기억하기)
> ![3](https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/c1afa33c-5f40-4df7-b514-ef327d3aeb7b)

```
df -k
```
> <img width="447" alt="4" src="https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/ad8d9043-d888-4501-bf43-956892f1aa75">


```
sudo mkfs /dev/sdf
```
><img width="463" alt="5" src="https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/e5ae3b26-4d31-48bf-b8a5-eb9073801afe">


```
sudo mkdir /appdir
```
> <img width="463" alt="6" src="https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/f737e1c0-487a-4ca7-a0e9-ab819fc758f1">


- 연결된 볼륨 /dev/sdf가 /dev/xvdf로 이름만 변경 되어 /appdir 디렉터리에 마운트(연결) 된 것을 볼 수있다.
```
sudo mount /dev/sdf /appdir
dk -k
```
> <img width="457" alt="7" src="https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/e1b93bde-837d-4a59-80e4-75953ad49af5">

- 마운트한 볼륨에 파일이나 디렉터리를 생성하여 사용할 수 있다.
```
cd /appdir
sudo touch  a b c d
sudo mkdir A B C D
ls
```
<img width="369" alt="8" src="https://github.com/hanmin0512/aws_EBS_Guide/assets/37041208/e5b0533d-6554-4315-9ef9-edbf2aed5795">

 - <a href= "https://github.com/hanmin0512/aws_EBS_Guide"> AWS EBS 구성 실습 github 보러가기</a>
