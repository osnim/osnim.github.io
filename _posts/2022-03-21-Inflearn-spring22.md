---
title: 스프링 입문 22 - 7.스프링 DB 접근 기술 (6)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-21 03:14:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 스프링 데이터 JPA (Java Persistence API)

이전 강의를 통해서 **스프링 부트**와 **JPA**만 사용해도 개발 생산성이 많이 증가하는 것을 확인할 수 있었습니다. 추가로 **스프링 데이터 JPA**를 사용하면, 기존의 한계를 넘어 리포지토리 구현 클래스 없이 인터페이스만으로 개발을 할 수 있습니다.<br>
개발해온 기본 CRUD 기능도 **스프링 데이터 JPA**가 모두 제공합니다. 따라서 개발자는 핵심 비즈니스 로직을 개발하는데, 집중할 수 있습니다.<br>
주의할 점은 **스프링 데이터 JPA**는 JPA를 편리하게 사용하도록 도와주는 기술입니다. 따라서 **JPA**를 먼저 학습한
후에 **스프링 데이터 JPA**를 학습을 권장합니다.

## 스프링 데이터 JPA 회원 리포지토리

![image](https://user-images.githubusercontent.com/79408217/159177190-8e6549d9-aecc-4b04-9d0d-aa7d2cbf3fc4.png)

**repository** 폴더에 **SpringDataJpaMemberRepository** 클래스가 아닌 **인터페이스**로 생성합니다.<br>
참고로 인터페이스가 인터페이스를 **상속** 받을 떄는 **implements**가 아니라 **extends**를 사용합니다.<br>
인터페이스는 **다중 상속**이 되기 때문에 **JpaRepository<Member, Long>, MemberRepository** 두개를 상속 받습니다.

```java
@Override
    Optional<Member> findByName(String name);
```

이렇게 하면 다른 메서드 추가 없이 JPA를 사용할 수 있습니다.

## 스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경

**스프링 데이터 JPA**가 **JpaRepository**를 상속 받고 있으면 구현체를 자동으로 만들어 줍니다. 그리고 **스프링 데이터 JPA**가 구현체인 **SpringDataJpaMemberRepository**를 스프링 빈에 자동으로 등록합니다. 저희는 그것을 손쉽게 가져다 사용하기만 하면 됩니다.

![image](https://user-images.githubusercontent.com/79408217/159177418-b5bc7f46-4d5b-4b89-84b2-104bb0accc36.png)

**SpringConfig**를 위와 같이 변경합니다.

### 테스트 실행

![image](https://user-images.githubusercontent.com/79408217/159177479-c494f8b2-3b82-4b95-8469-09e08a1b30cf.png)

Hibernate가 뜨며 정상적으로 작동하는 것을 확인할 수 있었습니다.

## 설명

![image](https://user-images.githubusercontent.com/79408217/159177740-a00a0e08-659f-4e65-8104-c7b93bada6ec.png)

![image](https://user-images.githubusercontent.com/79408217/159177571-f5091539-1b08-4965-8603-96d67c5c4973.png)

JpaRepository 인터페이스 내부에서 저희가 이전에 직접 구현한 기본 메서드가 이미 **findAll(), findAll(), findAllById()**로 구현되어 있는 것을 확인할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/159177656-1aecdb65-72b3-4c3e-97f4-abdf5f05ccf6.png)

CRUD에서도 기본 메서드가 이미 구현되어있는 것을 확인할 수 있었습니다.<br>
사실 여기에 맞추어 예제를 만든 것이라고 합니다.<br>
**findByName()** 만 추가한 이유는 공통으로 제공되는 메서드가 아니기 때문입니다.

![image](https://user-images.githubusercontent.com/79408217/159177915-2045acfa-3e06-416e-b5cb-e17db970b823.png)

그런데 추가로 코드로 내부를 구현을 하지 않고 findByName() 이름 하나만으로 findByName()을 구현할 수 있는 이유는 **스프링 데이터 JPA**가 **findBy** 뒤에 **Name**이라는 규칙을 통해서 자동으로 **JPQL**을 생성해주기 때문입니다.

따라서 **스프링 데이터 JPA**는 인터페이스를 통한 기본적인 CRUD를 제공하며, findByName(), findByEmail()처럼 메서드 이름 만으로 조회 기능 제공합니다.

실무에서는 **JPA**와 **스프링 데이터 JPA**를 기본으로 사용하고, 복잡한 동적 쿼리는 **Querydsl**라는 라이브러리를 사용합니다. <br>
**Querydsl**을 사용하면 쿼리도 자바 코드로 안전하게 작성할 수 있고, 동적 쿼리도 편리하게 작성할 수 있습니다.<br>
이 조합으로 해결하기 어려운 쿼리는 JPA가 제공하는 **네이티브 쿼리**를 사용하거나, 앞서 학습한 **스프링 JdbcTemplate**를 사용합니다.

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
