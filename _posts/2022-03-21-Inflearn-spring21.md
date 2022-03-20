---
title: 스프링 입문 21 - 7.스프링 DB 접근 기술 (5)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-21 01:10:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# JPA (Java Persistence API)

이전 강의에서 **순수 JDBC** 에서 **JDBC Template**으로 변경해서 작성해야 하는 코드를 확 줄일 수 있었습니다. 하지만 아직까지 SQL은 직접 작성해야하는 단점이 있었습니다. **JPA**를 사용하면 **SQL 쿼리**도 **JPA**가 자동으로 처리를 해주어 개발 생산성을 크게 높일 수 있습니다.<br>
마치 이전 **MemoryMemberRepository**에서 했던 것처럼 객체를 메모리에 넣듯이, JPA에 넣으면 JPA가 중간에 DB에 SQL 보내고, 데이터를 DB에서 가져오고 하는 것을 모두 처리해줍니다.<br>
JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환을 할 수 있다.<br>

## 라이브러리 추가

![image](https://user-images.githubusercontent.com/79408217/159173042-e4b2345a-118c-49d9-8eb9-0c04eec807bc.png)

**build.gradle 파일**에 이전에 추가했던 **implementation 'org.springframework.boot:spring-boot-starter-jdbc'** 를 주석 처리하고<br>
**implementation 'org.springframework.boot:spring-boot-starter-data-jpa'** 를 추가하고 <br>
**Load Gradle Changes**를 눌러줍니다. <br>

## 스프링 부트에 JPA 설정 추가

**application.properties** 파일에 다음을 추가합니다.

![image](https://user-images.githubusercontent.com/79408217/159173284-4e92d6e0-63d8-46f4-95f8-38a908cc5498.png)

**show-sql** : JPA가 생성하는 SQL을 출력합니다.<br>
**ddl-auto** : JPA는 DB 테이블을 자동으로 생성하는 기능을 제공하는데 **none** 를 사용하면 해당 기능을 끄게 됩니다. **none** 대신 **create**를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해줍니다.<br>
위 설정을 마치면 **External Libraries**에 **JPA**와 **hibernate**가 설치됩니다.<br>
**JPA**는 **자바 진영의 표준 인터페이스**이고 구현은 여러 업체들이나 벤더들이 제공을 하는데 저희는 주로 **JPA**의 **hibernate**의 인터페이스만을 사용한다고 합니다.<br>

JPA는 **객체**와 **ORM(Object Relational Mapping)**이라는 기술입니다.<br>
여기서 **ORM**은 **객체**와 **관계형 데이터베이스**를 **매핑**한다는 뜻입니다. 매핑은 어노테이션으로 진행합니다.<br>

## JPA 엔티티 매핑

![image](https://user-images.githubusercontent.com/79408217/159174238-989c9e9c-b8eb-4d4d-bd15-e8e5a4b91ec0.png)

**domain** 폴더의 **Member** 클래스에 **@Entity**를 추가하여 **Member**를 **JPA**가 관리하는 **Entity**로 만들어 줍니다.<br>
**PK(기본키)** **Id**라서 @Id 어노테이션을 추가합니다.<br>
DB가 알아서 id를 생성해주는데 이런 것을 **Identity**전략이라고 합니다. 따라서 **@GeneratedValue(strategy = GenerationType.IDENTITY)**

## JPA 회원 리포지토리

![image](https://user-images.githubusercontent.com/79408217/159174794-0b1d8ce1-88bf-4946-8feb-a0f25c0b1598.png)

**repository** 폴더에 **JpaMemberRepository** 클래스를 위와 같이 생성합니다.<br>
JPA 는 EntityManager를 통해서 동작합니다. build.gradle에서 jpa 라이브러리를 받았는데 스프링 부트가 자동으로 DB와 연결된 EntityManager를 생성합니다. 저희는 그 만들어진 EntityManager 주입(injection)만 받으면 됩니다.<br>

### save()

![image](https://user-images.githubusercontent.com/79408217/159174906-f5facbbd-a739-4c3c-94d6-838a31084ed4.png)

이전 **JdbcTemplate**으로 **save**를 구현한 것과 **JPA**를 이용하여 **save**를 구현한 코드의 차이입니다.<br>
**EntityManager**가 알아서 내부적으로 모든 것을 다 처리해주기 때문에 따로 **Insert**, **HashMap**등을 사용할 필요 없이 **EntityManager** 하나만 사용하면 됩니다.<br>
**persist**는 **영구 저장한다**는 의미입니다.

### findById()

![image](https://user-images.githubusercontent.com/79408217/159175167-a55e6274-a878-4c2d-b0a1-f57f93f39735.png)

**EntityManager**에 **find**라는 함수가 있습니다. 여기에 **타입**이랑 **PK(식별자)**만 넣어주면 됩니다.

### findByName()

![image](https://user-images.githubusercontent.com/79408217/159175783-c77a72b3-f301-4860-ae6a-8581921a772c.png)

**findByName()** 함수의 경우 테이블을 대상으로 쿼리를 날리는 것이 아니라 객체를 대상으로 쿼리를 날리는 것으로 알아서 SQL로 번역됩니다.<br>
따라서 **Member 엔티티**를 대상으로 쿼리를 날리는데, 특이한 점은 select 뒤에가 \*이나 id, name이 아닌 **m 객체** 자체를 조회합니다.<br>
**"select m from Member as m where m.name = :name", Member.class** 같은 **JPQL** 이라는 쿼리를 생성해서 조회해야 합니다.<br>
**Id** 처럼 **PK** 기반이 아닌 다른 컬럼으로 조회를 하는 경우 **JPQL**을 작성해야 합니다.

### findAll()

![image](https://user-images.githubusercontent.com/79408217/159175797-3a33d9aa-4d15-4c5e-aa14-5d5a00c47377.png)

**findByName()** 함수의 **JPQL** 쿼리와 내용이 유사합니다.

### JPA를 사용하도록 스프링 설정 변경

![image](https://user-images.githubusercontent.com/79408217/159176107-fbfadd97-97ad-463d-b28e-8aba4c0c269c.png)

**SpringConfig** 클래스에서 **JpaMemberRepository**를 사용하기 위해 설정을 변경합니다. 이전에 생성자였던 **dataSource**는 삭제합니다.

### 주의사항

![image](https://user-images.githubusercontent.com/79408217/159175939-1cdab715-d521-476e-ba5a-102972a9814e.png)

**JPA** 를 사용하려면 항상 **트랜잭션**이 필요합니다. 따라서 **서비스 계층**인 **MemberService** 클래스에 **@Transactional** 어노테이션을 추가합니다.

## 테스트 실행

![image](https://user-images.githubusercontent.com/79408217/159176237-a7a5ffbc-c8b8-418b-82a2-27eff2a9840a.png)

이전에 만든 **통합 테스트**로 **회원가입 기능**만 실행한 결과입니다. JPA를 구현하면 기본적으로 Hibernate라는 오픈소스 구현체가 사용됩니다.<br>
**select** 쿼리 문이 나간건 validation 때문에 하나 가져오는 것 같습니다.<br>
그 후 저희가 설정하지 않은 **Insert** 문을 **JPA**가 자동으로 실행하여 회원가입을 테스트 하는 것을 확인할 수 있었습니다.

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
