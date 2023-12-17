---
layout: single
title: "webdav php webshell 공격 "
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## metasploitable2_upload_webShell
metasploitable2 webdav서버에 웹쉘을 업로드하는 실습

## 실습 환경
- victim : Metasploitable2 Server (가상환경)
- hacker : KALI(가상환경) , Windows

## WebDav
- WebDAV는 HTTP 프로토콜을 기반으로 하는 확장 기능으로, 원격 웹 서버에 대한 파일 및 디렉터리 관리를 한다.
- 여러 웹 서버에서는 WebDAV를 활성화하면서 적절한 보안 설정을 하지 않으면 공격자가 무단으로 파일을 업로드하거나 시스템에 악의적인 파일을 배포할 수 있는 위험이 있는 취약점이다.

## 실습
> metasploitable2 서버 ip 확인
- ![meta_address](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/dd9b7b35-f079-44cb-8a72-58f67fbda9c6)

> dirb 모듈을 통한 metasploitable2 서버의 웹 디렉토리 구조 확인후 /dav/ 라는 경로를 발견.
-  ![dirb](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/13f287f7-357a-49ee-8ef6-040c4640f76b)

> dirb로 발견한 웹 디렉토리 경로의 취약점을 nikto 도구로 탐지.
> HTTP Method중 불필요한 OPTION, TRACE. DE:ETE. PROFIND, PROPPATCH, COPY, MOVE, LOCK, UNLOCK 메소드들을 허용하는 취약점 발견.
- ![nikto](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/98a24394-445e-4760-8bd5-d9fde7085c79)

> Burpsuite를 실행시켜 open browser 클릭
- ![burp_open_browser](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/bbfdbb0c-f332-43b7-b18f-be852b714bae)

> burp suite의 [proxy] -> [Intercept is on]을 활성화 해준다.
> open brwoser로 열린 브라우저의 주소창에 http://192.168.10.22/dav/ 입력
> 이후 burpsuite를 거친 http패킷을 본다. 
- ![intercept_on](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/73f839bc-b8aa-47be-9437-2c110aed9d32)

> nikto로 취약점을 확인한 결과 불필요한 메소드를 허용하기에 PUT메소드로 변경하여 파일을 업로드 한다. [forward] 클릭.
- ![tampering](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/4a95b7eb-e6bf-417d-bc51-c120f9156908)

> 패킷 전송 이후 웹페이지는 201 코드와 created라는 메시지와 함께 파일이 올라간것을 확인 가능.
- ![upload_shell_success](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/b9a8b37b-d118-4a26-a5c8-c912255cab95)
- ![uploading](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/29ff72c8-fb76-468e-a017-da5427627e48)

> webshell.php를 웹에서 실행하여 원격으로 metasploitable2 서버의 쉘 명령어를 실행할 수 있음을 확인.
- ![webshell](https://github.com/hanmin0512/metasploitable2_upload_webShell/assets/37041208/ebc22041-3309-4783-8ca6-d0c1be265cc6)

- <a href= "https://github.com/hanmin0512/metasploitable2_upload_webShell"> php webshell upload 공격</a>