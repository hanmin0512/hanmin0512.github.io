---
layout: single
title: "docker로 Load Balacing 구현해보기. "
categories: Security
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## docker_Load_balacner
docker에 생성한 볼륨과 네트워크를 연결하여 로드벨런싱을 직접 구현 해보자!

## 네트워크 3개를 생성
- docker 네트워크 드라이브 유형을 bridge로 설정.
- subnet주소 범위도 각각 설정하고 컨테이너에 할당될 ip주소 범위를 172.100.x.0/24 지정.
- gateway주소 172.100.x.1로 설정
- 네트워크 이름을 custom-net[N]으로 설정

```
sudo docker network create --driver=bridge --subnet 172.100.1.0/24 --ip-range 172.100.1.0/24 --gateway 172.100.1.1 custom-net01
sudo docker network create --driver=bridge --subnet 172.100.2.0/24 --ip-range 172.100.2.0/24 --gateway 172.100.2.1 custom-net02
sudo docker network create --driver=bridge --subnet 172.100.3.0/24 --ip-range 172.100.3.0/24 --gateway 172.100.3.1 custom-net03
```

## 볼륨을 3개 생성
- 호스트 로컬환경에 마운트할 볼륨 directory을 생성.

```
sudo docker volume create volume01
sudo docker volume create volume02
sudo docker volume create volume03
```

- 생성된 볼륨 directory 확인
```
sudo ls /var/lib/docker/volumes/
```

> <img width="500" alt="1" src="https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/1abc43ba-31b9-400a-94cc-4b8061df2067">


## 웹서버와 네트워크, 볼륨을 각각 연결
- -itd 옵션은 사용자가 컨테이너 내부에서 터미널 세션을 통해 명령어를 입력하고 결과를 볼 수 있게 해주며, 컨테이너 실행을 백그라운드로 실행 시킨다.
- -v 옵션은 호스트에 생성한 볼륨파일 volume[N]을 컨테이너의 /usr/share/nginx/html에 마운트 한다.
- --name은 컨테이너 명을 정의한다.
- --net은 생성한 네트워크를 할당한다
- --ip 옵션은 컨테이너의 고정 ip를 할당한다.
- nginx:1.18은 docker 이미지명 이다.

```
sudo docker run -itd -p 8001:80 -v volume01:/usr/share/nginx/html --name=web01 --net=custom-net01 --ip 172.100.1.100 nginx:1.18
sudo docker run -itd -p 8002:80 -v volume02:/usr/share/nginx/html --name=web02 --net=custom-net02 --ip 172.100.2.100 nginx:1.18
sudo docker run -itd -p 8003:80 -v volume03:/usr/share/nginx/html --name=web03 --net=custom-net03 --ip 172.100.3.100 nginx:1.18
```

## 로드밸런싱 구축

```
sudo vim /etc/nginx/nginx.conf
```

- "/etc/nginx/nginx.conf" 파일 수정
- 각각 포트에 가중치를 둬 라운드 로빈 방식의 로드밸런싱에서 비중을 설정한다.

```
http {

        upstream backend-lb{
                server 127.0.0.1:8001 weight=5;
                server 127.0.0.1:8002 weight=2;
                server 127.0.0.1:8003 weight=3;
        }

        server{
                listen 80 default_server;
                listen [::]:80 default_server;

                location / {
                        proxy_pass http://backend-lb;
                }
        }
}
```


## 마운트된 볼륨에 index.html 작성

- volume01

```
sudo vim /var/lib/docker/volumes/volume01/_data/index.html
```

- /var/lib/docker/volumes/volume01/_data/index.html 작성.

```
<h1> Load Balance page01</h1>
```

- volume02
```
sudo vim /var/lib/docker/volumes/volume02/_data/index.html
```

- /var/lib/docker/volumes/volume02/_data/index.html 작성.

```
<h1> Load Balance page02</h1>
```

- volume03
```
sudo vim /var/lib/docker/volumes/volume03/_data/index.html
```

- /var/lib/docker/volumes/volume03/_data/index.html 작성.

```
<h1> Load Balance page03</h1>
```

## 브라우저로 접속 시도.
> ![2](https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/c45f3070-a865-4822-b3de-a7d53660d746)
> ![3](https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/e5a410c2-718a-4be2-86f4-231dd75eb305)
> ![4](https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/ae0d1e56-2b03-4ca7-a9ba-bc41a8e0ebee)

## 삭제 작업.
- docker 컨테이너 stop

```
sudo docker stop $(sudo docker ps -q -f status=running)
```

> <img width="513" alt="5" src="https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/3e60f9d4-32f4-4c42-a6f3-7497ecf3d73a">


- docker 컨테이너 삭제

```
sudo docker rm $(sudo docker ps -q -f status=exited) 
```

> <img width="480" alt="6" src="https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/59d27673-19eb-400d-9af7-0a7ccd230609">


- docker 이미지 삭제

```
sudo docker images
sudo docker rmi nginx:1.18
sudo docker images
```

> <img width="754" alt="7" src="https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/ec2f4966-0f8e-4396-81f8-517773cac3b1">

- 나머지 삭제

```
sudo docker system prune -a
y
```

> <img width="554" alt="8" src="https://github.com/hanmin0512/docker_Load_balacner/assets/37041208/1cb9803c-a341-48b0-9c2a-8f81c2e49d54">


- <a href= "https://github.com/hanmin0512/docker_Load_balacner"> docker Load Balancer github 코드 보러가기</a>



