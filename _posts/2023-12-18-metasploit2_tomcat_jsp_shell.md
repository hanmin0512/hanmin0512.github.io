---
layout: single
title: "msfconsole tomcat_mgr_deploy공격 "
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

# metasploitable2_tomcat_jsp_shell
metasploitable2의 불필요한 포트 open 및 서비스 구동으로 인한 취약점을 이용한 웹쉘 공격

## 실습 환경
- victim : Metasploitable2 Server (가상환경)
- hacker : KALI(가상환경) , Windows

## 취약점
- Tomcat 5.5 취약점
- 불필요한 포트 열람
- 접근제어 미흡
- 특정 버전의 기본 계정 존재

## 실습
> nmpa으로 metasploitable 서버 ip를 이용하여 포트 및 서비스 스캐닝.
- ![nmap_scanning](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/e2262598-6fb9-4832-b9f0-849d31a9c299)

> tomcat 메인페이지 구조 확인.
- ![tomcat_mainPage](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/f26dce90-ca70-49fc-a36d-de5924168450)

> dirb를 활용하여 tomcat으로 서비스되고 있는 웹 디렉토리 구조 확인.
- ![dirb_tomcat_port](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/96c1b81a-c548-4446-8fc8-418cd92b8564)

> nikto로 웹 취약점 스캔
- ![nikto_scan](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/c8ab3915-24ea-4ed7-8131-434c919e06c5)

> 관리자 아이디 비밀번호가 기본 default인것을 확인. 관리자페이지로 이동하여 로그인.
- ![login_manager](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/b5fb8bd3-9f96-416f-9d45-bc4eba800875)

> war파일을 deployed 할 수 있는 페이지인 것을 확인.
> 준비해둔 웹쉘 war파일을 업로드
- ![before_war_deploy](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/f0b33cb6-d566-4f1e-8dda-58eaaa30ce01)

> deployed 된것을 확인
- ![After_uploading](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/f5ac46e3-3c4f-4a7f-ad0a-af76077f97f4)

> war파일이 deployed 되면 자동으로 톰캣에서 압축해제와 같은 개념으로 안에 파일들을  deployed 하게된다. 그래서 주소를 /[FILE_NAME.war]/shell.php 로 이동해야한다.
- ![In_War](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/3740cbb6-f470-4ece-8dcd-beacee31f37b)
- ![launch_command](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/3c180996-f3e7-407d-b954-4b82a9053f87)

> session이 끊기지 않도록, KALI 에서 nc를 이용해 포트 2개를 열고, 웹쉘에서 KALI로 연결을하는 reverse shell공격을 한다.
- ![KALI_nc_before](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/94467da1-288d-4897-8792-391bb27fe50d)

- ![webshelljsp](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/a8c65bf2-53a8-42bc-bba8-b8338bb597b2)

> 이후 reverse shell 공격이 성공하면 KALI 터미널 nc 5555포트 쪽에서 명령어를 입력하면 웹쉘로 이동하여 명령어 결과가 KALI의 6666포트 터미널에 출력된다.
- ![getShellnc](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/813d914d-32a6-4875-97d0-03aa7a75d09d)

## msfconsole을 활용한 실습.
> msfconsole -> search tomcat -> use 6
- ![use_module](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/075438c1-7e59-4a84-9eb3-6d3b04c4de6b)

> 옵션을 셋팅해준다.
- ![set_options](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/7b7d2506-f0ef-464a-9038-8cf4abb5d5a8)

> 셋팅 후 exploit을 해주면 알아서 취약점을 공략한 코드를 deployed 시키고 세션이 끊기면 undeployed를 시켜 웹서버에 올라간 파일은 삭제된다.
- ![get_shell](https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell/assets/37041208/da057126-2514-40d4-9a0e-5983026cc8d6)

- <a href= "https://github.com/hanmin0512/metasploitable2_tomcat_jsp_shell"> msfconsole tomcat_mgr_deploy</a>








