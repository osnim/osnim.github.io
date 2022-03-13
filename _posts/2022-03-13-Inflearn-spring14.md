---
title: 스프링 입문 14 - 5.스프링 빈과 의존관계 (1)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-13 16:08:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 컴포넌트 스캔과 자동 의존관계 설정

이전까지는 memberservice랑 리포지토리를 만들었습니다. 그리고 member객체를 만들어 **memberservice**를 통해서 가입을 진행시키고 리포지토리에 저장을 시키고 가져오는 로직을 구성하고 테스트까지 진행해봤습니다.<br>
이번시간에는 화면에 나타내기 위해 **Controller**랑 **BeutifulSoup**가 필요합니다. **Controller**랑 **BeutifulSoup**를 사용하기 위해서는 **MemberController**는 **MemberService**를 통해서 회원가입하고 **MemberService**를 통해서 조회할 수 있어야 합니다.<br>
이러한 관계를**MemberService**가 **MemberController**를 **의존**한다고 표현하고 둘은 **의존관계**에 있다고 표현합니다.<br>

## 스프링 빈 (Spring Bean)

**스프링 빈**: 스프링 컨테이너가 관리하는 자바 객체로 스프링에서는 객체를 빈이라고 표현합니다.<br>
**@**의 기능 : 스프링 부트가 실행되면 **스프링 컨테이너**에 **MemberController** 객체를 생성하고 컨테이너에 넣어서 **스프링**이 관리를 합니다. 이러한 것을 **스프링 빈**이 관리된다고 표현합니다.

## MemberController 생성

![image](https://user-images.githubusercontent.com/79408217/158062017-d019244c-171a-4f53-a2ea-17464c57b914.png)

**Controller** 패키지에 **MemberController**라는 클래스를 생성하고 @Controller라는 어노테이션을 달아줍니다.

**MemberController**에서 **MemberService** 객체를 **new**로 생성해서 **MemberService**를 사용할 수도 있지만 스프링을 사용하면 스프링 컨테이너에 등록하고 컨테이너에서 가져와서 사용해야 합니다.<br>
이유는 **Memberservice** 객체는 **MemberController** 뿐만 아니라 다른 **Controller**에서 **MemberService**를 가져다 써야하는데 **MemberService**의 기능은 특별하지 않습니다. 그래서 **Controller**별로 구분지어서 새로 만들 필요가 없이 하나의 **MemberService**를 컨테이너에 등록해서 사용하는 것이 훨씬 효율적입니다.<br>

컨테이너에 등록하는 방법은 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/158061982-86c60ee5-b0ca-4be2-b5a8-4fcd0cf1e6f1.png)

생성자를 이용해서 **memberService** 객체를 선언하고 생성자에 **@Autowired** 어노테이션을 추가합니다. 이렇게 되면 생성자에 있는 **@Autowired**로 인해 스프링이 **memberService**를 컨테이너에 있는 **memberService**를 가져다가 연결을 시켜줍니다.

## @Autowired 에러

![image](https://user-images.githubusercontent.com/79408217/158062429-9bf9fc50-012d-48c7-88fd-040ea46d4317.png)

하지만 이렇게 에러가 발생하는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/158062788-57520282-0846-4611-8330-e288727c530d.png)

이유는 아직 **MemberService**가 컨테이너에 등록되어 있지 않기 때문입니다.

![image](https://user-images.githubusercontent.com/79408217/158062489-45476246-6ec0-40e7-88ab-3b3c7aca30fd.png)

위는 **MemberService** 클래스 입니다. 위에 아무런 **@(어노테이션)**이 없어서 **MemberService**의 **@Autowired**의 에러가 발생한 것입니다. 이를 고치기 위해 **@Service**를 **MemberService** 클래스 위에 추가합니다.

![image](https://user-images.githubusercontent.com/79408217/158062638-a99fdecd-8063-4725-96d6-3dc2182d2c3b.png)

추가로 **MemoryMemberRepository** 클래스로 이동해서 **@Repository**를 추가합니다.

지금 추가한 **어노테이션**은 **@Controller**, **@Controller**, **@Repository** 총 3개입니다. 컨트롤러를 통해서 외부요청을 받고, 서비스에서 비즈니스 로직을 만들고, 리포지토리에서 데이터를 저장하는 가장 정형화된 패턴으로 이러한 형식으로 자주 개발을 합니다.

## DI(Dependency Injection), 의존성 주입

![image](https://user-images.githubusercontent.com/79408217/158063351-91eca645-d0fd-4b74-b49b-57b8621268ae.png)

스프링의 가장 큰 장점중 하나인 의존성 주입입니다. **MemberController**를 생성할 때, 스프링 빈에 등록되어있는 **MemberService** 객체를 **MemberController**에 파라미터로 직접 넣어줍니다. 이를 **의존성 주입**이라고 하는데 **MemberController**는 가만히 있고 외부(스프링)에서 의존관계를 넣어주는 것입니다.

![image](https://user-images.githubusercontent.com/79408217/158062906-826c36e6-abee-4b3b-bf6d-f4a5e178d500.png)

![image](https://user-images.githubusercontent.com/79408217/158063468-d7dca554-7cbc-4290-9b9f-1a1b401fe338.png)

**memberService**는 **memberRepository**가 필요하기 때문에 **MemberService** 클래스에 가서 **memberService**생성자에 **@Autowired**를 추가합니다.<br>
또한 **memberService**는 **memberRepository**가 필요하기 때문에 **memberRepository**를 파라미터로 넣어줍니다.

main 매서드를 실행하면 에러가 발생하지 않는 것을 확인할 수 있습니다.

## 스프링 빈을 등록하는 2가지 방법

1. **컴포넌트 스캔과 자동 의존관계 설정**

   **어노테이션**을 이용해서 **@Controller**, **@Service**, **@Repository**를 추가한 것이 **컴포넌트 스캔** 방식입니다.

   ![image](https://user-images.githubusercontent.com/79408217/158063925-88cb61b8-94bf-4b86-a638-f9ea8169427e.png)

   위 그림은 **@Controller**, **@Controller**, **@Repository**를 차례로 확인한 것입니다. 이를 통해 모두 **@Component** 어노테이션이 자동으로 있는 것을 확인할 수 있습니다. 이를 통해 스프링이 시작하고 **@Component** 가 있는 것을 **스캔**해서 **@Component**가 있는 객체를 생성해서 컨테이너에 저장을 시킵니다.<br>
   **의존 관계**는 **@Autowired**을 이용해 자동으로 연결시켜 줍니다.

2. **자바 코드로 직접 스프링 빈 등록하기**

   다음시간에 자세히 다뤄 볼 것입니다.

## 주의사항

1.  **main** 메서드가 있는 **Hello.Hellospring** 패키지 안에 **컴포넌트**만 스캔하여 스프링 빈에 등록합니다.<br>
    **Hello.Hellospring** 패키지 밖에서 선언된 **컴포넌트**들은 스캔할 수 없습니다.

2.  스프링은 컨테이너에 스프링 빈을 등록할 때, **싱글톤**을 기본으로 하여 등록합니다.

![image](https://user-images.githubusercontent.com/79408217/158062906-826c36e6-abee-4b3b-bf6d-f4a5e178d500.png)

**싱글톤**이란 딱 한 개만 등록한다는 말로 위에서는 **helloController**는 딱 한개만 등록한다는 의미입니다. 유일하게 한 개만 등록해서 공유한다는 말로 만약 **memberService**와 **OrderService(가정)**가 모두 **memberRepository**를 의존한다면 모두에게 같은 **memberRepository** 인스턴스를 반환합니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
