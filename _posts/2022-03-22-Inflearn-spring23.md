---
title: 스프링 입문 23 - 8.AOP (1)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-22 13:56:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# AOP(Aspect Oriented Programming)

**AOP**란 **Aspect Oriented Programming**의 약자로 **관점 지향 프로그래밍**이라고 합니다.

## 시간 측정 로직

먼저 AOP에 대한 자세한 설명을 바로 시작하지 않고 다음과 같은 상황을 가정해보겠습니다.

![image](https://user-images.githubusercontent.com/79408217/159412000-00fff44a-f8f0-4520-84a7-10b027a69b9a.png)

- 만약 모든 메소드의 호출시간을 측정하고 싶다면 어떻게 해야할까?
- 각 메서드 마다 **System.currentTimeMillis()** 를 이용하여 끝 - 시간을 계산하면 됩니다.

![image](https://user-images.githubusercontent.com/79408217/159412325-69f01746-4c47-4e59-a310-babdcc5c68b9.png)

코드로 시간 측정 로직을 구현하면 다음과 같습니다. 예를 들어 join 메서드의 시간을 측정하고 싶다면 위와 같이 변경해야합니다.<br>

### 위 코드의 문제점

- 만약 메서드가 약 몇 천 ~ 몇 만 단위로 커진다면 이렇게 코딩을 하는 것을 매우 비효율적입니다.
- 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아닙니다.
- 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
- 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.
- 시간을 측정하는 로직은 공통 관심 사항입니다.

## AOP 적용

![image](https://user-images.githubusercontent.com/79408217/159412770-47c3a0c7-9852-4035-94ae-79116891025e.png)

**AOP**의 핵심은 **공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리**하는 것입니다.<br>
모든 메서드에 시간 측정 로직을 구현하는 것이 아니라 **시간 측정 로직만 따로 분리**시켜 원하는 메소드에 적용하는 것입니다.<br>
스프링에서는 AOP 기술을 쉽게 적용할 수 있게 제공을 합니다.

### AOP 코드

![image](https://user-images.githubusercontent.com/79408217/159414292-ab3ae542-4640-4791-a578-43971a5f3c87.png)

**aop**라는 패키지를 만들고 그 안에 **TimeTraceAop** 클래스를 생성합니다. 그 후 **@Aspect** 를 추가합니다. **AOP** 기능을 사용하기 위해서는 **@Aspect**가 필요합니다.
AOP 코드는 메뉴얼을 그대로 따라합니다.

```java
@Around("execution(* Hello.Hellospring..*(..))")
```

위 코드는 Hello.Hellospring 패키지 하위에 이 AOP를 모두 적용시킨다는 의미입니다.

![image](https://user-images.githubusercontent.com/79408217/159414256-f1d15bf3-c4c6-44be-ac19-41332a3dee54.png)

위 코드를 통해 어떻게 메서드를 들어가고 어디서 병목현상이 발생했는지 바로 확인할 수 있습니다.

### AOP의 핵심

```java
  public Object execute(ProceedingJoinPoint joinPoint)
```

위 코드에서 joinPoint를 통해 AOP를 조작할 수 있습니다.

### AOP 결과

- 회원가입, 회원 조회등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리하여 시간을 측정하는 로직을 별도의 공통 로직으로 만들었습니다.
- 핵심 관심 사항을 깔끔하게 유지할 수 있으며 변경이 필요하면 이 로직만 변경하면 됩니다.

## 스프링의 AOP 동작 방식 설명

![image](https://user-images.githubusercontent.com/79408217/159415339-049106dd-db08-4d11-92b6-c77ea4392c43.png)

![image](https://user-images.githubusercontent.com/79408217/159415380-1d1c5bc3-f688-49bf-a96b-4b16a3ef818a.png)

스프링에서는 **AOP**가 적용되면 **가짜 memberService**를 생성합니다 이것이 **프록시 memberService** 라고 합니다. 그리고 스프링 컨테이너에 **memberService** 스프링 빈을 등록할 때, **가짜 memberService** 스프링 빈을 실행하고 그 실행이 완료되면 **joinPoint.proceed()**를 실행하여 다음 **진짜 memberService**을 실행합니다.<br>
즉, **helloController**가 호출하는 것은 **프록시** 기술로 생성한 가짜 memberService 입니다.

**AOP**는 스프링이 **DI** 기능을 지원해줘서 가능합니다. 스프링이 알아서 **DI**를 해주기 때문에 직접 설정해줄 필요없이 받아만 쓰는 메서드 입장에서는 프록시인지 진짜인지 구별 안하기 때문입니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /
- getter, setter 단축키 : Alt + Insert
- 생성자가 1개인 경우 **@Autowired** 생략 가능

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
