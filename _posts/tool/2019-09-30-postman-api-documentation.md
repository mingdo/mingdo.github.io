---
layout: post
title:  "Postman API Documentation"
date:   2019-09-30 06:15:00
author: Mingdo
categories: tool
tag: postman
---

# Postman 기반 API 문서화
Postman API Documentation 기능을 사용해 API 스펙 문서를 만들어 보자.

## API Documentation
### Collection 생성
- New > Collection
![](/img/tool/postman/create_collection.png)

### Request 생성
- New > Request
- 위에서 생성한 Test Collection 에 저장  
![](/img/tool/postman/create_request.png)

### Example 작성
- Examples > Add Example
![](/img/tool/postman/add_example.png)
 
### API Documentation 확인
collection 과 request 만 작성하면 웹에서 API 문서를 확인할 수 있다.
- Collection menu > View in web
![](/img/tool/postman/api_docs.png)

### API Documentation Publish
API 문서를 게시하면 생성된 URL 을 통해 누구나 API 문서에 접근 가능하다.
- API Documentation > Publish
![](/img/tool/postman/publish.png)
pro 또는 enterprise 계정을 사용하면 도메인을 지정해 접근을 제한할 수 있다.[Publish Guide](https://learning.getpostman.com/docs/postman/api_documentation/publishing_public_docs/)

### Team Workspace
API 문서를 함께 작성해야 하는 팀원들을 Team Workspace 를 만들어 초대하면 공동 작업이 가능하다.
- Postman Workspace > Team > Create New
![](/img/tool/postman/create_team.png)
- 작업 중이던 Personal Workspace import
![](/img/tool/postman/import_workspace.png)
- Team Workspace API Documentation 확인
![](/img/tool/postman/team_documentation.png)


## Postman 추가 기능
### Path variable
- Collection menu > Edit > variables
path-variable 을 사용할 경우 Collection 변수를 지정해 URL 표기 가능
![](/img/tool/postman/set_collection_variable.png)
- request 변경
![](/img/tool/postman/reqeust_path_variable.png)
- example 변경
![](/img/tool/postman/example_path_variable.png)
- web API 문서 변경 확인
![](/img/tool/postman/result_path_variable.png)

### Environment variable
환경 변수를 사용하면 local, develop, stage, master 변수 설정을 달리 할 수 있다.
- local environment 추가
![](/img/tool/postman/local_env.png)
- develop environment 추가
![](/img/tool/postman/develop_env.png)
- request 변경
![](/img/tool/postman/request_env.png)
- response 변경
![](/img/tool/postman/example_env.png)
- web API 문서 변경 확인  
local environment 확인
![](/img/tool/postman/result_env_local.png)  
develop environment 확인
![](/img/tool/postman/result_env_develop.png)

### Runner
Postman 의 Runner 를 사용하면 Collection 에 대한 테스트가 가능하다.  
Runner 를 실행해 호출하고 싶은 API 를 선택한 뒤 Run Test 를 누르면 된다.
Runner 를 Jenkins 와 연동([Runner Guide](https://learning.getpostman.com/docs/postman/collection_runs/integration_with_jenkins/))해 테스트 자동화도 가능하다.
- Collection menu > Run
![](/img/tool/postman/runner.png)

## 추가 정보
### Mock Server
Postman Mock Server 기능을 사용해 API 요청에 대한 임시 응답을 받을 수 있다.
위에서 작성한 요청은 실제 서버에 올라가지 않았기 때문에 제대로 동작하지 않는다.
이 때 Mock Server 를 구성하고 이에 대한 응답을 작성하면 실제 API 를 호출한 것과 같은 효과를 얻을 수 있다.
- [Mock Server Guide](https://learning.getpostman.com/docs/postman/mock_servers/intro_to_mock_servers/)

### 비용
Public API Documentation 을 사용하거나 Team Workspace 를 일정 수준 이상으로 사용하기 위해선
Pro 또는 Enterprise 계정으로 업그레이드 하는 것이 좋다.
- [Pricing Guide](https://www.getpostman.com/pricing?_ga=2.100861768.832564590.1569302881-1619312319.1569302881)

### 현재 사용량 확인
- Postman Web > Resource Usage
- [Usage Page](/img/tool/postman/usage.png)
