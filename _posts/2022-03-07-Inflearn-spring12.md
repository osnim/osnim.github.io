---
title: 스프링 입문 8 - 4.회원 관리 예제 - 백엔드 개발 (4)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-07 17:49:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 회원 서비스 개발

회원 리포지토리와 도메인을 활용해서 실제 비즈니스 로직을 작성하는 단계입니다. <br>
서비스 클래스는 비즈니스에 가까운 이름으로 지어야 개발자와 기획자 모두 로직을 쉽게 파악할 수 있습니다.

## 회원가입 기능

```java
/** 회원가입 **/
    public Long join(Member member){
        // 조건: 같은 이름을 가진 회원은 가입 X
        Optional<Member> result = memberRepository.findByName(member.getName());
        result.ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        });

        memberRepository.save(member);
        return member.getId();
    }
```

**Hello.Hellospring** 패키지에 **service**라는 패키지를 하나 생성하고 그 안에 **MemberService**라는 클래스를 하나 생성합니다.<br>
그 후 **MemoryMemberRepository()** 객체를 하나 생성합니다.<br>
**result**가 **Optional** 이라서 여러 메소드를 사용할 수 있는데 그 중 **ifPresent** 값이 존재하면 확인을 합니다.<br>
만약 중복이라면 **IllegalStateException**가 작동해서 예외처리를 해줍니다.

```java
    memberRepository.findByName(member.getName())
        .ifPresent(m -> {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
    });
```

**memberRepository.findByName**은 이미 **Optional**로 선언되었기 때문에 간결하게 코드를 작성하고 싶으면 **Optional**을 지우고 바로 **ifPresente**를 사용합니다.

![image](https://user-images.githubusercontent.com/79408217/157005417-dccfeb05-e318-4af0-8494-ddd3b24157a8.png)

![image](https://user-images.githubusercontent.com/79408217/157005709-38bd971d-19ca-4adb-84d0-a0f64e741300.png)

그 후 **memberRepository.findByName**의 코드 블록을 <br>
**드래그 한 후 우클릭 > Refactor > Extract Method** 를 눌러 **vaildateDuplicateMember**라고 메서드 이름을 바꿔줍니다.<br>
이를 통해 **join** 메소드는 중복을 검증하고 바로 멤버를 저장합니다.

## 회원 조회 기능

```java
 /** 전체 회원 조회 **/
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }
```

**MemberService**클래스에 **findMembers()**를 선언하고 List를 **java.uilt** 클래스로 import합니다.<br>
추가로 다음 메소드를 **findMembers()** 밑에 넣습니다.

```java
public Optional<Member> findOne(Long memberId){
        return memberRepository.findById(memberId);
    }
```

![image](https://user-images.githubusercontent.com/79408217/157011150-6fb70952-b6fe-4ce2-b97f-9b94e7620257.png)

전체 코드입니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
