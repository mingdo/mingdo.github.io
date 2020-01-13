---
layout: post
title:  "[JAVA-02] JVM 구조"
date:   2019-01-08 23:28:00
author: Mingdo
categories: java
---

## 클래스 로더

클래스 로더는 .class 파일 바이트 코드를 읽어 JVM 메모리에 저장한다.

내부적으로 로딩, 링크, 초기화 순으로 작업이 이루어진다.

### 로딩

class 파일을 읽어 바이너리 데이터를 만들고 메모리의 메소드 영역에 저장한다. 이 때 저장되는 정보는 아래와 같다.

- 패키지 경로를 포함한 클래스명
- 클래스 메소드와 변수
- class? interface? enum?

로딩이 끝나면 해당 타입의 Class 객체를 생성하여 메모리의 힙 영역에 저장한다.



클래스 로더는 계층구조로 이루어져 있다. 기본적으로 3 가지 제공되는데 그 종류는 아래와 같다.

- Bootstrap Class Loader
  - JAVA_HOME\lib에 있는 코어 자바 API를 제공한다. 최상위 우선순위를 가진 클래스 로더
  - 네이티브 코드로 구현
- Platform Class Loader
  - JAVA_HOME\lib\ext 폴더 또는 java.ext.dirs 시스템 변수에 해당하는 위치에 있는 클래스를 읽는다.
- Application Class Loader
  - 애플리케이션 클래스패스에서 클래스를 읽는다.

Bootstrap Class Loader -> Platform Class Loader -> Application Class Loader 순으로 클래스를 읽는다.

Application Class Loader 에서도 해당 클래스를 읽지 못하면 ClassNotFoundException 이 발생한다.

### 링크

Verify, Prepare, Resolve 3 단계로 이루어져 있다.

- Verify
  - .class 파일 형식이 유효한지 검사한다.
- Prepare
  - 클래스 변수 (static 변수) 와 기본값에 필요한 메모리 준비
- Resolve (optional)
  - 심볼릭 메모리 레퍼런스를 메소드 영역에 있는 실제 레퍼런스로 교체
  - 객체 A 안에서 객체 B 를 참조한다고 해도 실제로는 논리적으로만 참조를 하는데 Resolve 하면 실제 레퍼런스로 교체된다.

### 초기화

static 변수의 값을 할당한다. static 블럭이 있다면 이때 실행된다.





## 메모리

JVM 의 메모리는 5가지 영역으로 구분된다.

- 메소드
  - 클래스 수준의 정보 저장
  - 클래스명, 풀 패키지명, 메소드, 변수
  - 공유 자원 (다른 영역에서 참조 가능)
- 힙
  - 실제 인스턴스 저장
  - 공유 자원 (다른 영역에서 참조 가능)
- 스택
  - 스레드마다 런타임 스택 생성
  - 스레드 종료하면 런타임 스택 소멸
  - 메소드 호출을 런타임 스택 안에 스택 프레임이라는 블럭으로 쌓음
- PC
  - 스레드 마다 생성
  - 런타임 스택에서 현재 실행할 스택 프레임을 가리키는 포인터
- 네이티브 메소드 스택
  - 스레드 마다 생성
  - 네이티브 메소드 호출할 때 사용하는 스택





## 실행엔진

바이트 코드를 읽어 실행한다.

- 인터프리터
  - 한 줄 씩 바이트 코드를 네이티브 코드로 컴파일 한 뒤 실행
- JIT 컴파일러
  - 반복적인 코드를 JIT 컴파일러에 보내 미리 네이티브 코드로 변환
  - 변환된 네이티브 코드 인터프리터가 바로 사용
- GC (Garbage Collector)
  - 참조되지 않는 객체를 모아 정리





## JNI (Java Native Interface)

- 네이티브 메소드를 사용하기 위한 인터페이스





## 네이티브 메소드 라이브러리

- c, c++ 을 통해 구현된 메소드
