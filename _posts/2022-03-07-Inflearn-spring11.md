---
title: 스프링 입문 11 - 4.회원 관리 예제 - 백엔드 개발 (3)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-07 13:49:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 회원 리포지토리 테스트 케이스 작성

### 테스트 케이스란?

이전 시간에 작성한 **MemberRepository 인터페이스**, **MemoryMemberRepository** 등이 제대로 작동하는지 코드로 검증하는 방법으로 개발한 기능을 실행해서 테스트 할 때 자바의 main 메서드를 통해서 실행하거나, 웹 애플리케이션의 컨트롤러를 통해서 해당 기능을 실행합니다.

하지만 이러한 방법은 오래 걸리고, 반복 실행하기 어렵고 여러 테스트를 한번에 실행하기 어렵다는 단점이 있습니다.<br>
자바는 **JUnit**이라는 프레임워크로 테스트를 실행해서 이러한 문제를 해결합니다.

## JUnit을 통한 TestCase 작성

![image](https://user-images.githubusercontent.com/79408217/156980104-6d1c51c6-9d47-4138-a9cd-a3fa57f3b51a.png)

이전 시간에 **main** 디렉토리에 **repository** 패키지를 만든 것 처럼, **test** 디렉토리에 **repository**라는 패키지를 새로 생성합니다.<br>
관례적으로 테스트를 실행할 패키지나 클래스의 이름 뒤에 Test 를 붙이는 방식으로 테스트 케이스를 작성합니다.<br>
이번 시간은 **MemoryMemberRepository**을 테스트 할 것입니다. <br>
따라서 test > repository 에 **MemoryMemberRepositoryTest** 라는 클래스를 새로 생성합니다.

**MemoryMemberRepositoryTest** 클래스는 굳이 **Public**으로 **접근지정자**를 설정하지 않아도 됩니다.

**MemoryMemberRepositoryTest** 클에스에 다음과 같은 코드를 작성합니다.

```java
class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");
        repository.save(member);
    }
}
```

### save Testcase

**@Test** 는 **Junit**에서 import 한 것입니다. 먼저 member객체를 만들고 그 member객체 이름을 spring으로 정하고 repository에 save했습니다. 이후 repository가 제대로 작동하는지 확인하기 위해 member의 getId()로 id를 찾았습니다.

![image](https://user-images.githubusercontent.com/79408217/156973552-dbf0b662-8dc6-4e1c-a69a-631b32297671.png)

**save** 함수를 다시 살펴보면 id 는 ++sequence로 1씩 증가하는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/156973689-fb28ad88-5f23-4c18-baf8-d9d6f896a48c.png)

```java
class MemoryMemberRepositoryTest {
    MemoryMemberRepository repository = new MemoryMemberRepository();

    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");
        repository.save(member);
        Member result = repository.findById(member.getId()).get();
    }
}
```

**findById**는 반환형이 **Optional**이므로 **get()**이라는 함수를 통해서 꺼낼 수 있습니다.<br> **get()**으로 꺼낸 값을 **result**에 저장합니다.

```java
    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");
        repository.save(member);
        Member result = repository.findById(member.getId()).get();
        Assertions.assertEquals(member, result);
    }
}
```

**result**에 값이 제대로 들어왔는지 확인하기 위해 **JUnit**에서 제공하는 **Assertions**의 **assertEquals** 함수를 사용하여 **save**함수를 **run** 합니다.

#### - Assertions에서 값이 일치하지 않는 경우

![image](https://user-images.githubusercontent.com/79408217/156974999-e600f80e-f370-457c-89d6-2e0200b17ae8.png)

기댓값과 실제값이 달라 에러가 발생하는 것을 확인할 수 있었습니다.

#### - Assertions에서 값이 일치하는 경우

![image](https://user-images.githubusercontent.com/79408217/156974853-c1988740-731a-48f4-a837-391a463e6f49.png)

로그가 아무것도 찍히진 않지만 왼쪽에 모두 초록색 체크가 나오며 에러가 발생하지 않았다는 것을 확인 할 수 있었습니다.

### org.assert.core.api의 Assertions 사용

요즘은 **org.assert.core.api의 Assertions**를 더 많이 사용합니다.<br>
**Assertions.assertThat()**함수의 자세한 사용은 다음과 같습니다.

```java
    @Test
    public void save(){
        Member member = new Member();
        member.setName("spring");
        repository.save(member);
        Member result = repository.findById(member.getId()).get();
        //Assertions.assertEquals(member, result);
        Assertions.assertThat(member).isEqualTo(result);
    }
}
```

![image](https://user-images.githubusercontent.com/79408217/156975636-6ddf910f-c60d-482e-b508-6c51df25ee24.png)

**Add on-demand static imoort**를 사용하면 **assertThat**함수를 바로 사용할 수 있습니다.

### findByName Testcase

```java
@Test
    public void findByName(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        Member result = repository.findByName("spring1").get();
        assertThat(result).isEqualTo(member1);
    }
```

![image](https://user-images.githubusercontent.com/79408217/156977009-651c91ef-9ef2-4499-be12-a86db1682e66.png)

`spring1`, `spring2` 라는 회원이 가입을 했다고 케이스를 작성하고 **assertThat(result).isEqualTo(객체이름)**으로 **findById**함수를 **run**하여 test 해보았습니다.

여기서 spring 1을 2로 바꾸거나 member1을 2로 바꾸는 경우 에러가 발생하는 것을 확인할 수 있습니다.

또한 각 함수별로 작동시키지 않고 Class로 묶어서 함께 test를 진행할 수도 있습니다.

### findAll Testcase

```java
@Test
    public void findAll(){
        Member member1 = new Member();
        member1.setName("spring1");
        repository.save(member1);

        Member member2 = new Member();
        member2.setName("spring2");
        repository.save(member2);

        List<Member> result = repository.findAll();
        assertThat(result.size()).isEqualTo(2);
        assertThat(result).isEqualTo(3);//에러
    }
```

### 주의사항

![image](https://user-images.githubusercontent.com/79408217/156978712-816dd921-13bf-4b2e-a988-2f0304cb8377.png)

이렇게 코드를 작성할 경우 **MemoryMemberRepositoryTest**의 함수들이 순서대로 test가 진행되지 않고 무작위로 진행됩니다. 또한 **findByName()**에서 선언한 객체가 Repository에 그대로 남아있어 **findAll()**에서 테스트 할 경우 이름이 겹치는 객체가 존재하여 에러가 발생합니다.

이를 고치기 위해 매 test마다 **repository**를 **clean하는 함수**를 작성해야 합니다.

#### Repository clean 함수

**MemoryMemberRepositoryTest** 클래스 맨 위에 다음과 같은 코드를 작성합니다.

```java
@AfterEach
    public void afterEach(){
        repository.clearStore();
    }
```

**@AfterEach**는 **@Test**하는 메서드가 끝날때 마다 실행하는 메서드로 **save(), findByName(), findAll()** 테스트가 끝날때마다 한번씩 실행됩니다.

이를 위해 **MemoryMemberRepository** 에 다음과 같은 메서드를 추가합니다.

```java
public void clearStore(){
        store.clear();
    }
```

clear()함수를 통해 저장된 store를 비웁니다.

![image](https://user-images.githubusercontent.com/79408217/156979373-e6304309-6db4-4628-8725-30361cab4a0f.png)

**@AfterEach**를 통해 에러 없이 정상적으로 test가 실행되는 것을 확인할 수 있었습니다.

## TDD 테스트 주도 개발

오늘 한 테스트케이스 작성을 나중에 하는 것이 아니라 테스트케이스를 먼저 만들고 이에 맞는 함수나 메서드, 클래스를 코딩하는 개발 방법론으로 테스트 주도 개발, TDD(Test-Driven-Development)이라고 합니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
