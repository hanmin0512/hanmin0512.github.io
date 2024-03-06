---
layout: single
title: "AWS Lambda 실습"
categories: Cloud
toc: true
author_profile: false
sidebar:
  nav: "docs"
---

## AWS Lambda 실습
aws S3 lambda hand on tutorial 따라하기

## lambda_function code
```
import boto3
import os
import sys
import uuid
from urllib.parse import unquote_plus
from PIL import Image
import PIL.Image
            
s3_client = boto3.client('s3')
            
def resize_image(image_path, resized_path):
  with Image.open(image_path) as image:
    image.thumbnail(tuple(x / 2 for x in image.size))
    image.save(resized_path)
            
def lambda_handler(event, context):
  for record in event['Records']:
    bucket = record['s3']['bucket']['name']
    key = unquote_plus(record['s3']['object']['key'])
    tmpkey = key.replace('/', '')
    download_path = '/tmp/{}{}'.format(uuid.uuid4(), tmpkey)
    upload_path = '/tmp/resized-{}'.format(tmpkey)
    s3_client.download_file(bucket, key, download_path)
    resize_image(download_path, upload_path)
    s3_client.upload_file(upload_path, '{}-resized'.format(bucket), 'resized-{}'.format(key))
```

## 파이썬 파일 생성한 디렉터리에서 작업
```
mkdir package
pip install \
--platform manylinux2014_x86_64 \
--target=package \
--implementation cp \
--python-version 3.9 \
--only-binary=:all: --upgrade \
pillow boto3
```

- 위 명령어들을 수행해준 후 다음 작업
```
cd package
zip -r ../lambda_function.zip .
cd ..
zip lambda_function.zip lambda_function.py
```

## bucket 만들기
- source bucket만들기
> ![스크린샷 2024-01-20 오후 8 06 50](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/baf62489-5aa4-47fc-9e97-d07759d820ed)

- 이후 모든 값은 default값으로 두고 버킷 생성하기를 클릭한다

- destination bucket 만들기
> ![스크린샷 2024-01-20 오후 8 08 54](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/beb86e14-6eff-4454-b458-39eb75fdbe85)
- 소스코드가 소스버킷 이름 뒤에 "-resized"가 추가된 버킷에 썸네일 이미지를 저장하므로 이름을 소스버킷 뒤에 "-resized"를 붙혀준다.
- 이후 모든 값은 default값으로 두고 버킷 생성하기를 클릭한다


## 정책 생성
- IAM > 정책 > 정책 생성
> ![스크린샷 2024-01-20 오후 8 14 20](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/a7392c2c-f30a-4762-bb2c-3184a702a54f)

```
- JSON 코드
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents",
                "logs:CreateLogGroup",
                "logs:CreateLogStream"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::*/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::*/*"
        }
    ]
}
```
- 다음 클릭
> ![스크린샷 2024-01-20 오후 8 16 52](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/271724d6-49e9-4b10-8d9b-909f9320d137)
- 정책 생성

## 역할 부여
- IAM > 역할 > 역할 생성
> ![스크린샷 2024-01-20 오후 8 18 13](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/41d948d3-148e-4dbf-a2ce-877a1b401fe7)
- 다음
- 이 역할에 위에 만든 정책을 부여
> ![스크린샷 2024-01-20 오후 8 19 35](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/9b44b511-6ebc-428e-bb33-58eceb75f428)
- 다음
> ![스크린샷 2024-01-20 오후 8 20 59](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/0bcc0ef8-0e3f-4946-85a9-370287ad6c00)
- 이후 값은 건들지 않고 역할 생성 클릭

## lambda 함수 생성
- lambda > 함수 > 함수 생성
> ![스크린샷 2024-01-20 오후 8 24 16](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/033a7013-1fcc-4227-b7b4-69d6abb67e53)
> ![스크린샷 2024-01-20 오후 8 24 28](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/922550ad-8498-4b18-9cbf-ff900721bb87)
- 함수 생성

> ![스크린샷 2024-01-20 오후 8 27 00](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/77e41796-84d6-4a8f-a196-2399406b7855)

> ![스크린샷 2024-01-20 오후 8 27 35](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/6e7c41a2-66f3-4f1e-bb5d-01b1620738e3)
- 저장
- 트리거 추가
> ![스크린샷 2024-01-20 오후 8 32 13](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/56d99529-9146-43d2-8ec3-e527a51c0784)

> ![스크린샷 2024-01-20 오후 8 34 30](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/e602b252-8606-4880-a647-1cbcc2af98f8)

- 추가

## s3 이미지 올려 썸네일 만들어 지는지 확인하기
- source 버킷에 이미지 올린다.
> ![스크린샷 2024-01-20 오후 8 36 08](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/b08b70ae-3801-4d3c-bf9d-38dd75557d3f)

- ~~-resized 버킷에 썸네일 이미지가 생성된것을 볼 수 있다.
> ![스크린샷 2024-01-20 오후 8 37 09](https://github.com/hanmin0512/aws_Lambda_Guid/assets/37041208/44df086b-107b-40da-9dec-fd523ab1967e)

 - <a href= "https://github.com/hanmin0512/aws_Lambda_Guide"> AWS Lambda 실습 github 보러가기</a>