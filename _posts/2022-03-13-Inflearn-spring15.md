---
title: 스프링 입문 15 - 5.스프링 빈과 의존관계 (2)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-13 23:56:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 자바 코드로 직접 스프링 빈 등록하기

이전시간에 등록한 **@Service**, **@Repository**, **@Autowired**를 모두 지우고 진행합니다.<br>
**Hello.Hellospring** 패키지에 SpringConfig라는 클래스를 생성하고 **@Configuration**를 추가합니다.

![image](https://user-images.githubusercontent.com/79408217/158066407-cb5c8c8b-851b-43be-8c0b-196dc79eeae0.png)

그 후 **@Bean** 어노테이션과 함께 **memberService** 를 추가합니다. **@Bean**이 **memberService**를 컨테이너에 저장한다는 의미입니다.<br>
**memberService** 외에 **MemoryMemberRepository** 객체를 반환하는 **memberRepository** 메서드를 생성하고 **@Bean**을 추가합니다.<br>
**MemberService()**는 **memberRepository**에 의존하기 때문에 **MemberService** 객체를 반환할 때 파라미터로 **memberRepository()** 메서드를 넣어줍니다.<br>

![image](https://user-images.githubusercontent.com/79408217/158066580-2f85fd85-2284-4c18-8422-ccad56aac3c2.png)

위 과정을 마치면 의존관계 설정이 완료됩니다.

## DI의 3가지 방법

1. 필드 주입

   ![image](https://user-images.githubusercontent.com/79408217/158066828-e9f28e95-beec-4b63-9b7e-d9be92293d60.png)

   필드 주입이란 **MemberController** 생성자 부분을 모두 지우고 필드에 **@Autowired**를 직접 입력하는 방법입니다. 필드 주입 방식은 변환시킬수 없기 때문에 크게 추천하는 방식은 아닙니다.

2. setter 주입

   ![image](https://user-images.githubusercontent.com/79408217/158066961-c8fe5b35-7b11-4e74-b1b8-dbc886540bde.png)

   **Generator**에서 **MemberService setter**를 생성하고 **@Autowired**를 추가합니다. setter를 통해 의존관계가 주입됩니다. <br>
   **setter 주입**은 누군가가 **MemberController**를 호출했을 때 public으로 열려있어야 합니다. 그런데 **public**이라는 접근 지정자를 사용해서 중간에 다른 곳에서 호출되면 문제가 발생할 수도 있기 때문에 권장하는 방법은 아닙니다.

3. 생성자 주입
   생성자 주입이란 **MemberController**에서 생성자를 통해서 **memberService**가 **MemberController**에 주입이 됩니다. 의존관계가 프로그램 동작중에 동적으로 변하는 경우는 없으므로 위 3가지 방법중에서 생성자 주입 방식을 권장합니다.

## 자바 코드로 직접 스프링 빈 등록하기의 장점

상황에 따라 구현 클래스를 변경해야 하면 설정을 통해 스프링 빈으로 등록이 가능합니다.

![image](https://user-images.githubusercontent.com/79408217/158067464-0b839a2d-2ec2-4838-b679-afbba433437f.png)

클래스의 의존관계에서 볼수 있듯이 지금은 DB가 없고 DB와 연결되어있지 않아서 가상으로 구현체(**MemoryMemberRepository**)를 생성하고 후에 교체하는 방식으로 스프링을 진행하고 있습니다.<br>
하지만 나중에는 **MemoryMemberRepository** 다른 DB나 리포지토리로 바꿀 경우 기존의 코드는 거의 수정하지 않고 교체할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/158067613-06495a56-81b8-4d21-b4ab-98f4d6b7c878.png)

예를 들어 **SpringConfig** 에서 **MemoryMemberRepository()**를 **DBMemberRepository()**로 변경하기만 하면 됩니다.

## 참고

- 실무에서는 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 컴포넌트 스캔을 사용합니다.

- @Autowired 를 통한 DI는 **helloConroller** , **memberService** 등과 같이 스프링이 관리하는
  객체에서만 동작합니다. 스프링 빈으로 등록하지 않고 내가 직접 생성한 객체에서는 동작하지 않습니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
