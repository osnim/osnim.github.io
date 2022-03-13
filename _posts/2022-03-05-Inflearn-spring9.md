---
title: 스프링 입문 9 - 4.회원 관리 예제 - 백엔드 개발 (1)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-05 22:03:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 비즈니스 요구사항 정리

데이터: 회원 ID, 이름
기능: 회원 등록, 조회

이번 백엔드 개발은 자바 스프링의 생태계를 배우기 위한 것이라서 매우 단순하게 요구사항을 정하였습니다.

## 일반적인 웹 어플리케이션 계층 구조

![image](https://user-images.githubusercontent.com/79408217/156884405-4986e32b-4435-41bf-8715-2c40763b47ea.png)

- 컨트롤러: 웹 MVC의 컨트롤러 역할
- 서비스: 핵심 비즈니스 로직 구현되어있습니다.
  - 예시
  1. 회원은 중복가입 X
- 리포지토리: 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리
- 도메인: 비즈니스 도메인 객체, 예)회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

## 클래스 의존관계

![image](https://user-images.githubusercontent.com/79408217/156884601-73085c16-01e7-4e62-a708-0950b1ee21b6.png)

- MemberService: 회원 비즈니스 로직을 담당합니다.
- MemberRepository: 회원을 저장하는 것은 인터페이스로 설계합니다. 이유는 아직 데이터 저장소가 정해지지 않아서 임시로 메모리 구현로 데이터를 만들고 이후 데이터 저장소가 정해지면 그 저장소로 데이터 바꾸기 위해 인터페이스로 설계합니다.
- MemoryMemberRepository: 아직 데이터 저장소가 선정되지 않아서 구현체로 가벼운 메모리 기반의 데이터 저장소 사용합니다

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 내용을 직접 정리한 것입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
