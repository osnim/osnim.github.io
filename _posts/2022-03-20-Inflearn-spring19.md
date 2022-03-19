---
title: 스프링 입문 19 - 7.스프링 DB 접근 기술 (3)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-20 01:29:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 스프링 통합 테스트

이번 시간은 이전 시간에 만든 DB와 스프링이 JDBC로 연결된 코드를 모두를 테스트 할 것입니다.

이전 코드를 살펴보면, 스프링과 관련 없는 자바 코드입니다. 하지만 이전 코드는 스프링이 직접 관리해주는 코드입니다. 따라서 이번 테스트는 스프링과 연관하여 진행해보겠습니다.

## 테스트 클래스 작성

이전에 만든 **MemberServiceTest** 클래스를 복사하여 **MemberServiceIntegrationTest**로 새로운 클래스를 생성합니다.

그 후 **@SpringBootTest**, **@Transactional** 을 추가합니다.

- **@SpringBootTest** : 스프링 컨테이너와 테스트를 함께 실행합니다.
- **@Transactional** : 테스트를 실행할 때, 트랜잭션을 먼저 실행하고 DB의 데이터를 넣고 커밋을 하고 DB를 저장합니다. 그 후 롤백을 통해 반영을 시키지 않고 테스트 이전 상태로 돌려줍니다.

![image](https://user-images.githubusercontent.com/79408217/159130777-2387dd12-014e-4536-9c68-029fd000f86c.png)

**MemoryMemberRepository()**를 사용하여 메모리 객체를 이용하는 부분은 지우고 필드 주입 방식으로 **@Autowired** 어노테이션을 바로 달아줍니다. 테스트는 공개하는 것이 아니기 때문에 필드 주입을 해도 크게 문제가 되지 않습니다.<br>
**AfterEach**는 **@Transactional**가 있으므로 지워줍니다. 뿐만 아니라 **setup(), findMembers(), findOne()**도 지워줍니다.

## 회원가입() 테스트 실행

![image](https://user-images.githubusercontent.com/79408217/159130914-49594357-57d2-4b18-b647-0c346172712e.png)

먼저 실험삼아 **회원가입()** 기능을 테스트 해보았습니다. 결과는 에러가 발생하는데 이유는 **spring**이라는 이름이 DB에 남아있는데 테스트로 가입을 시도해보았기 때문입니다.

![image](https://user-images.githubusercontent.com/79408217/159130961-fae93839-d2bf-4bd4-ab93-15c8b3119a2a.png)

따라서 DB의 내용을 모두 지우고 다시 테스트 진행해보겠습니다. 그 전에 **@Transactional**를 주석 처리하고 실행해보겠습니다.

![image](https://user-images.githubusercontent.com/79408217/159131116-e5ef42fd-3707-4863-a1d0-b3537c6b3ced.png)

테스트로 회원가입을 진행한 경우 `spring`이라는 이름을 가진 회원이 등록된 것을 확인할 수 있습니다.

하지만 두 번 실행할 경우 spring이 메모리에 남아있기 때문에 지워줘야 합니다. 이를 전에는 AfterEach와 BeforeEach가 지워줬다면 지금은 **@Transactional**이 지워줍니다.

즉 **@Transactional**이 있는 상태는 데이터가 남아있지 않아 다음 테스트에 영향을 주지 않습니다.

## 전체 단위 테스트

![image](https://user-images.githubusercontent.com/79408217/159131579-e2adfdc2-db7b-4017-aeb2-97fa2d60fa15.png)

테스트 전체를 실행해도 에러가 발생하지 않는 것을 확인할 수 있습니다.

## 추가

먼저 이전 테스트를 **단위 테스트** 라고 하고 이번 테스트는 DB까지 연동하여 **통합 테스트** 라고합니다.<br>
스프링 자체에서 테스트 기능을 제공한다고 이전의 테스트는 필요가 없는 것은 아닙니다. <br>
이유는 먼저 속도의 차이에서 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/159131676-acb68eff-4d00-4fb1-a130-19de87387203.png)

이전 스프링과 함께 통합 테스트를 하는 시간의 약 1/7로 확 줄어든 것을 확인할 수 있었습니다.<br>
또한 단위 테스트 처럼 기능 단위로 쪼개서 테스트 하는 것이 더 좋은 테스트일 확률이 높습니다. 그래서 스프링 컨테이너 없이 테스트 할 수 있도록 훈련을 해야합니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /
- getter, setter 단축키 : Alt + Insert

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
