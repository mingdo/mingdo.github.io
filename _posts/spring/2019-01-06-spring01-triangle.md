---
layout: post
title:  "[Spring01] Spring 핵심 3대 요소"
date:   2019-01-06 23:28:00
author: Mingdo
categories: spring
---

Spring 핵심 3대 요소인 IoC, AOP, PSA 에 대한 정리

# IoC
### IoC (Inversion of Control)
IoC 란 제어역전으로 내가 사용할 인스턴스의 생성을 내가 아닌 외부에서 하겠다는 의미이다.
Spring 에서는 IoC Container 를 통해 이를 관리한다.
Bean 은 IoC Container 에서 관리하는 객체를 의미한다.
Bean 으로 등록하는 방법에는 두 가지가 있다.
첫 번째는 객체에 @Component annotation 을 달아 Component Scanning 의 대상으로 만드는 것이다.
Scanning 은 @ComponentScan annotation 을 통해 수행되며 해당 annotation 이 있는 위치의 하위 패키지들의 class 를 대상으로 Component Scanning 을 한다.
두 번째는 직접 XML 또는 JAVA Configuration 설정 파일에 Bean 으로 등록하는 것이다.
Bean 을 사용하는 방법은 생성자를 통해 주입받는 방법, @Autowired annotation 을 사용하는 방법, ApplicationContext 의 getBean() method 를 사용하는 방법이 있다.

### IoC Container
Bean Factory 로 Bean 을 관리해주는 객체이다.
ApplicationContext 가 이를 수행한다.
의존성의 주입은 IoC Container 가 수행하는데 Container 에 등록된 Bean 들 간의 주입만 가능하다.

### DI (Dependency Injection)
DI 란 의존성 주입으로 인스턴스 내부에서 의존적인 인스턴스를 사용할 수 있게 가져오는 것이다.
의존성을 주입할 수 있는 대상은 constructor, setter, field 세 가지다.
@Autowired annotation 을 통해 의존성을 주입한다.
이 중 권고되는 방법은 constructor 에 의존성을 주입하는 것으로 의존성 주입 없이는 인스턴스 생성이 불가능하기 때문이다.
A 가 B 를 의존하고 B 가 A 를 의존하는 순환참조의 경우 constructor 방법 대신 setter 나 field 에 의존주입을 해야한다.
근본적으로는 그러한 참조가 일어나지 않도록 설계하는 것이 좋다.

# AOP
관심사를 한 곳으로 모은다. 흩어져 있는 코드를 뭉친다.
### AOP (Aspect Of Programming)
아래와 같은 소스가 있다.
```
class A {
	public static void aaa() {
    	System.out.println("aaa fungtion running");
    }
    
    public static void bbb() {
    	System.out.println("bbb fungtion running");
	}
}

class B {
	public void ccc() {
    	A.aaa();
        System.out.println("AOP Test 1");
        A.bbb();
    }
    
    public void ddd() {
    	A.aaa();
        System.out.println("AOP Test 2");
        A.bbb();
    }
}
```

위와 같이 class B 의 method ccc(), ddd() 는 시작과 끝에 class A 의 method 를 호출한다.
중간 과정이 다를 뿐 method 의 시작과 끝에는 중복코드가 발생한다.
아래 코드는 흩어져 있는 중복코드를 한 군데로 모은 것이다.
```
class A {
	public static void aaa() {
    	System.out.println("aaa fungtion running");
    }
    
    public static void bbb() {
    	System.out.println("bbb fungtion running");
	}
}

class B {
	public void eee(Sting aopTestJob) {
    	A.aaa();
        System.out.println(aopTestJob);
        A.bbb();
    }
}
```

eee method 하나로 ccc(), ddd() 로 수행하던 작업이 가능하다.
AOP 란 이렇게 관심사를 한 데 모으는 프로그래밍 기법이다.
AOP 를 구현하는 방법에는 여러가지가 있다.
컴파일 단계에서 조작해 java 파일에는 소스가 없지만 class 파일에는 소스가 존재하도록 하는 방법 (AspectJ),
class 파일을 읽어 메모리에 올릴 때 바이트 코드를 조작하는 방법 (AspectJ),
Spring AOP 에서 사용하는 Proxy 패턴을 사용하는 방법이 있다.

### Proxy Design Pattern
Proxy 패턴은 기존코드를 수정하지 않고 새로운 기능을 추가하는 패턴이다. 예를 들어 아래와 같은 class 와 method 가 있을 때
```
class D {
	public static void dFunc() {
        System.out.println("Origin Function Running");
    }
}
```
Proxy 패턴은 위의 소스를 수정하지 않고
```
class DProxy {
	public static void dFunc() {
    	System.out.println("Proxy Job 1");
        d.dFunc();
        System.out.println("Proxy Job 2");
    }
}
```
새로운 Proxy class 를 생성해 필요한 Job 을 실행하는 것이다. 기존 기능은 그대로 유지하면서 다른 기능을 덧 붙인 것이다.
Spring AOP 는 이러한 Proxy 패턴을 사용하는데 사용자의 소스 수정 없이 내부적으로 필요한 소스를 덧 붙인 뒤 수행한다.
그 예로 @transactional annotation 이 있다.
트랜잭션을 실행하기 위해서는 실제 쿼리 전 후로 시작과 종료를 수행하는 소스를 작성해야 하는데 @transactional annotation 을 붙이면 내부적으로 Proxy 패턴을 통해 해당 scope 전 후에 트랜잭션 소스를 추가해준다.
물론 이러한 작업은 Spring AOP 내부적으로 수행된다.

### Annotation 을 통한 AOP 구현
Spring 에서는 annotation 을 만들어 필요한 작업을 묶어둘 수 있다.
annotation 을 만든 뒤 @Target 으로 대상체를 지정하고 @Retention 으로 lifecycle 을 지정한다.
해당 annotation 은 아직 아무런 기능도 하지 않는다.
annotation 에서 수행할 작업은 @Aspect annotation 을 통해 지정한다.
@Aspect 를 지정한 class 내부에 @Around annotation 을 통해 어떠한 annotation 의 앞 뒤에서 작업을 수행할 지 지정할 수 있다.
그 외에 Bean 에 대한 작업을 지정하는 것도 가능하니 AOP 에 대한 자세한 학습이 필요하다.

# PSA
### PSA (Portable Service Abstraction)
PSA 란 잘 만든 인터페이스로 Spring MVC, Spring Transaction, Spring Cache 와 같이 한 서비스를 사용할 때 low level 을 신경쓸 필요를 없애 준다.
예를 들어 tomcat 기반의 Spring MVC 를 구현한 뒤 이를 netty 기반의 Spring Webflux 로 변경해도 내가 짠 소스의 수정 없이(100% 호환되지 않는 경우도 있음) 패키지만 변경해주면 된다.
Spring Transaction 의 경우 Transaction Manager 에 상관 없이 내가 짠 소스는 동일하다.
Spring 에서 제공해 주는 Service 를 사용하면 되기 때문이다.

