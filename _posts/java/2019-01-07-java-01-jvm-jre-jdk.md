---
layout: post
title:  "[JAVA-01] JVM, JRE, JDK 에 대해"
date:   2019-01-07 23:28:00
author: Mingdo
categories: java
---

## JVM ?

JVM (Java Virtual Machine) 은 자바 가상 머신으로 바이트 코드로 이루어진 클래스 파일을 운영체제에 맞게 변환하여 실행해준다.

운영체제에 맞게 네이티브 코드로 변환해주기 때문에 플랫폼에 종속적이다.

실제 바이트코드를 실행하는 시점에서 자바 가상 머신이 인터프리터로 읽고 반복되는 코드는 JIT (Just-In-Time) 컴파일을 통해 네이티브 코드로 변환해 인터프리터가 바로 실행할 수 있게 한다.

바이트 코드를 실행하는 표준이자 특정 벤더 (Oracle, Amazon etc.) 가 만든 구현체다.

추가로 JVM 은 클래스 파일로 컴파일할 수 있다면 자바가 아닌 다른 언어로 작성됐어도 실행 가능하다. (완벽하게 호환되지는 않음)

## JRE ?

> JRE = JVM + Library

JRE (Java Runtime Environment) 는 자바 어플리케이션을 실행할 수 있는 배포판으로 JVM 과 핵심 라이브러리, 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 포함한다.

## JDK ?

> JDK = JRE + Development-Tool

JDK (Java Development Kit) 는 자바 런타임 환경과 개발에 필요한 도구들 (jar, javac etc.) 을 포함한다.

소스코드를 사용할 때 작성하는 자바 언어는 플랫폼에 독립적이다.

## Java ?

자바는 언어 자체를 의미하기도 하고 JDK 가 포함하고 있는 모든 것들을 통칭하기도 한다.

### Java Program 실행

1. Java 언어로 .java 파일 작성
2. JDK 에 포함된 자바 컴파일러 (javac) 로 .java 파일 컴파일 => .class 파일 (바이트 코드) 생성
3. .class 파일 java 명령어로 실행 => JVM 이 실행 => 인터프리터가 바이트 코드 읽은 뒤 실행
4. 프로그램 동작
