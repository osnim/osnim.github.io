---
title: 스프링 입문 - 3.라이브러리 살펴보기
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-02-23 12:50:00 +0900
categories: [Inflearn]
tags: [Inflearn, Spring]
---

## 라이브러리 살펴보기

![image](https://user-images.githubusercontent.com/79408217/155259261-fc80aba1-a329-423e-8296-7b8c54f69ace.png)

External Libraries를 통해 Gradle이 알아서 당겨온 라이브러리를 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155259513-6c111890-027c-4035-a094-73af09107d0c.png)
![image](https://user-images.githubusercontent.com/79408217/155259619-8d4350d4-e0ca-49ac-9a2e-1ebb5fd12d16.png)

IntelliJ에서 왼쪽 하단의 네모 아이콘을 누르면 오른쪽 상단에 Gradle 메뉴가 생성되는것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155259873-5f28be97-17ea-4875-9edd-4b4e66c7d091.png)

여기서의 Dependencies가 라이브러리의 의존관계를 나타내며 저는 초기 spring boot starter를 통해 web와 thymeleaf 라이브러리만 가져왔으나 web와 thymeleaf이 필요한 라이브러리는 알아서 가져온 것을 확인할 수 있습니다.
즉 web 라이브러리가 tomcat 라이브러리를 Embeded 하고 있습니다. 요즘에는 예쩐과 달리 tomcat 서버, WAS를 따로 설치하지 않고 이런 방식으로 라이브러리 하나만을 빌드해서 웹서버를 띄어 편하게 개발을 진행하고 있습니다.

(`*`) 은 중복을 제거하여 나타낸것을 표시합니다.

또한 스프링 부트 라이브러리를 쓰면 스프링 코어까지 알아서 한번에 다 가져와서 사용할 수 있게 만들어 줍니다.

## log

![image](https://user-images.githubusercontent.com/79408217/155263021-ce6d1610-e474-40ad-9d86-c6b5b1b40d4f.png)

현업에서 system.out.println()을 사용하지 않아야 하고 log로 꼭 출력을 해야합니다. log로 남겨야 심각한 에러를 확인할 수 있고 로그파일을 관리할 수 있게 된다.
따라서 스프링 부트에서도 logging을 표준으로 설정하고 log할 수 있는 라이브러리를 알아서 가져와 준다.

## test와 관련된 라이브러리

![image](https://user-images.githubusercontent.com/79408217/155263107-05600bfd-0de2-47e8-bf29-4dfbc7bca91b.png)
junit: test를 편하게 하게 도와주는 라이브러리

## 정리 스프링 부트 라이브러리

#### spring-boot-starter-web

- spring-boot-starter-tomcat: 톰캣 (웹서버)
- spring-webmvc: 스프링 웹 MVC

#### spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View)

#### spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅

- spring-boot

  - spring-core

- spring-boot-starter-logging
  - logback, slf4j

## 테스트 라이브러리

#### spring-boot-starter-test

-junit: 테스트 프레임워크

-mockito: 목 라이브러리

-assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리

-spring-test: 스프링 통합 테스트 지원

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
