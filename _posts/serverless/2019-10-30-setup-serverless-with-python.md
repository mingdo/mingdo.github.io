---
layout: post
title:  "Serverless 프레임워크를 사용한 AWS Lmabda Function 배포"
date:   2019-10-30 00:00:01
author: Mingdo
categories: serverless
tag: serverless, python
---

Serverless 프레임워크를 사용해 aws lambda function 을 배포하는 방법에 대한 정리

# 로컬 설치가 필요한 언어 및 도구
- python 3.7
- npm 6.12.0
- pip 19.3.1
- virtualenv 16.7.5
- Serverless 1.51.0 (function 단위 배포를 위해 버전 맞춰야 함)

# 프로젝트 셋업
### serverless 프로젝트 생성
```
$ serverless create \
--template aws-python3 \
--name my-first-serverless \
--path my-first-serverless
```
serverless.yml 파일로 배포할 함수 지정 및 함수 실행 방식 등을 설정할 수 있다.

### 패키지 관리를 위한 설정
프로젝트 폴더로 이동  
```
$ cd my-first-serverless
```
virtualenv 구성 및 실행
- 로컬 환경과 상관없이 패키지들을 관리하기 위해 가상환경 설정  
```
$ virtualenv venv --python=python3
$ source venv/bin/activate
```
npm 구성  
```
$ npm init
```

### 기본 패키지 설치
serverless-python-requirements 설치  
- lambda function 이 실제 컴파일되는 OS 로 compile 할 수 있게 도와준다.
- requirements.txt 에 있는 패키지들을 묶어서 배포해준다.  
```
$ npm install --save serverless-python-requirements
```
serverless-offline 설치
- 로컬에서 lambda function 배포한 뒤 테스트 가능하도록 도와준다.  
```
$ npm install —-save serverless-offline
```
serverless.yml 수정
- 파일 끝에 아래 내용 추가  
```
plugins:
  - serverless-python-requirements
  - serverless-offline

custom:
  pythonRequirements:
    dockerizePip: non-linux
```

### lambda function 배포
##### 로컬 테스트
serverless.yml 수정
- http events 주석제거  
```
 59 functions: 
 60   hello: 
 61     handler: handler.hello 
 62 #    The following are a few example events you can configure 
 63 #    NOTE: Please make sure to change your handler code to work with those events 
 64 #    Check the event documentation for details 
 65     events: 
 66       - http: 
 67           path: users/create 
 68           method: get
```

serverless offline start  
```
$ sls offline start
```
api 호출  
```
$ curl -X GET http://localhost:3000/users/create
```
결과 확인

##### 배포
```
$ sls deploy -v
```
endpoint 확인 뒤 실행 및 결과 확인