---
layout: single
title: "ICMP[Fragment] flood공격, IDS탐지 룰 정의 및 탐지 검증 실습 "
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## IDS_ICMP_Detection
- ICMP flood공격을 IDS로 탐지하기

## 실습 환경
- 공격자 [Kali]
- 피해자 [metasplotable server]
- IDS [Security Onion -suguil]

## ICMP Flood(Fragment)공격 시그니처 분석하기

```
hping3 icmp --rand-source [Dst IP] -d [packet length] --flood
```

- 공격하기
> ![hping](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/1bd0ebd9-acd8-4736-99f6-7500c011b638)

- wireshark를 통해 데이터 분석
> ![wireshark](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/b770eb7c-88bb-4117-9b74-428df4d073b5)

- 데이터 부분에 16비트 이후 부터 58585858 반복 되는 시그니처를 발견

## 분석한 시그니처로 침입탐지 룰 정의하기
- security onion - suguil 룰 설정
> ![lcal_ruies](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/8cf5cb46-f07f-4dbe-9e9e-b826a91a90a7)

```
alert icmp any any -> any any (msg:"Ping Of Death"; sid:3000007; threshold:type both, track by_src, count 50, seconds 2; content:"|585858|"; offset:15; rev:4;)
```

>![rule](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/05bd6f63-801d-496c-81a8-fb9e5c546f3c)

## 침임 탐지 검증
- suguil에 탐지 되는 것을 확인
> ![suguilfull](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/9aaf84db-97b3-448d-a057-8fe43eb7a3ae)

- Ping Of Death로 탐지된 패킷 자세히 보기
> ![suguil](https://github.com/hanmin0512/IDS_ICMP_Detection/assets/37041208/a8af76c7-4692-44dc-8fa9-7f98af3c8e10)
> 마지막 ip.dst가 16.79.101.175 패킷은 공격할 때 와이어 샤크에 스니핑 된것을 보아 올바르게 패킷이 탐지 된것을 알 수 있다.

## github 보러가기
- <a href= "https://github.com/hanmin0512/IDS_ICMP_Detection"> IDS 관제룰 (github)</a>