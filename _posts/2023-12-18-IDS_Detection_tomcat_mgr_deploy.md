---
layout: single
title: "tomcat_mgr_deploy공격, IDS탐지 룰 정의 및 탐지 검증 실습 "
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# IDS_Detection_tomcat_mgr_deploy
msfconsole의 tomcat_mgr_deploy 공격 시그니처를 분석하고 snort룰을 정의하여 sguil로 탐지하기

## 실습 환경
- victim : Metasploitable2 Server (가상환경)
- hacker : KALI(가상환경)
- IDS : security onion-[sguil]

## 공격 시그니처 패턴 분석.
> 칼리리눅스 msfconsole의 tomcat_mgr_deploy를 실행시켜 공격을 실행 하여 wireshark로 패킷 분석.
> 분석 결과 : data부분에 504b030414로 시작하는 것을 확인 (여러번 공격하여 PUT 메소드인 부분의 데이터를 여러개 확인.)
- ![kalicap](https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy/assets/37041208/c9c3a33a-6cdd-4604-b2af-fdd4957417b2)

> 분석한 공격 시그니처 패턴에 맞게 룰을 정의한다. <br>
> ``` alert tcp any any -> any any (msg:"Detect PUT Data"; content:"|504b030414|"; nocase; sid:300010;) ```
- ![rules](https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy/assets/37041208/bfa677c3-a0b8-4a3a-8b85-b036210c728a)

> 룰 업데이트 하기.
> ``` # rule-update ```
- ![rule-update](https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy/assets/37041208/03a1ee9c-39e1-44eb-b429-e048f4da6fac)

> IDS 탐지 확인하기
- ![캡처](https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy/assets/37041208/8ebbdd30-b6b7-4aae-9454-78a921e175c2)

> 탐지된 패킷 detail view
- ![detail](https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy/assets/37041208/88e0cf54-5943-4d54-a3dd-66449a9854f6)

- <a href= "https://github.com/hanmin0512/IDS_Detection_tomcat_mgr_deploy"> tomcat_mgr_deploy 공격 IDS 탐지</a>
