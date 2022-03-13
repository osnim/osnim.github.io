---
title: 스프링 입문 13 - 4.회원 관리 예제 - 백엔드 개발 (5)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-07 19:11:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 회원 서비스 테스트

이전시간에 만든 회원 서비스를 테스트 해보겠습니다. 이번시간에는 지난 번 테스트 케이스를 작성하는 것과 달리 **MemberService** 안에서 바로 테스트를 진행할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/157016081-d0b66178-39fd-4cb7-b8c9-dc22138f1a57.png)

![image](https://user-images.githubusercontent.com/79408217/157016317-ba329ebb-4c48-4eb5-a54b-57a6c4384245.png)

**MemberService**를 선택 > Alt + Enter > Create Test > 멤버 모두 체크<br>
이렇게 하면 자동으로 TestCase가 자동으로 생성됩니다.

테스트는 **한글**로 바꿔도 가능합니다. 빌드할 때 실제 코드에 반영되지 않고 영어권 사람들과 작업하는 것이 아니라면 직관적인 한글로 테스트를 하는 경우도 있으니 이번에는 한글로 메서드를 바꿔서 테스트 해보겠습니다.

## 회원가입 서비스 테스트

다음과 같이 코드를 작성하고 run을 실행합니다.

```java
@Test
    void 회원가입() {
        //given
        Member member = new Member();
        member.setName("hello");

        //when
        Long saveId = memberService.join(member);

        //then
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());

    }
```

**assertThat**은 org.assertj.core.api.Assertions에서 import static 한 메서드 입니다.

테스트를 보통 3구간으로 나누어서 실험합니다.<br>
//given: 주어진 값들을 나타냅니다.<br>
//when: 조건이 들어간 부분입니다.<br>
//then: 테스트 결과부분을 나타냅니다.<br>

이 부분은 회원가입에서 예외 없는 부분을 테스트 한 경우입니다. 테스트에서는 예외처리가 잘 작동하는지도 중요하기 때문에 회원가입에서 중복으로 가입이 되는 지 알아보도록 하겠습니다.

## 중복 회원 가입 예외처리 테스트

### try catch 문

먼저 이름이 같은 경우에서 join을 2번 해보겠습니다. 그 후 try catch문으로 예외 처리를 합니다.

```java
@Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        // vaildateDuplicateMember(member)가 발생해야함
        try {
            memberService.join(member2);
            fail("예외가 발생해야합니다.");
        }catch (IllegalStateException e){
            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
        }

        //then
    }
```

위와 같이 테스트 코드를 작성하고 실행을 해보았습니다.

![image](https://user-images.githubusercontent.com/79408217/157021992-49c848cb-ac55-4c2a-ab9d-dafb67659e0f.png)

**vaildateDuplicateMember(member)**가 제대로 작동하여 에러가 발생하지 않았고 **isEqualto**를 통해 똑같이 에러 메세지가 같다는 것을 확인할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/157022212-75d15b67-f4e1-439b-9bf7-13689b4fe352.png)

만약 예외처리를 잘못 했다면 다음과 같은 메세지가 출력됩니다.

### assertThrows 사용

```java
@Test
    public void 중복_회원_예외(){
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");

        //when
        memberService.join(member1);
        assertThrows(IllegalStateException.class, () -> memberService.join(member2));
```

![image](https://user-images.githubusercontent.com/79408217/157023529-aec9ddc2-3c44-4100-8237-f3675266b4c3.png)

**assertThrows()**를 사용하기 위해서는 **(static org.junit.jupiter.api.Assertions.)\***; 클래스를 import 해야합니다.

이는 **() -> memberService.join(member2)** 구문이 실행되면 **IllegalStateException.class**가 실행되어 예외를 처리하는 구조입니다.

에러 메세지를 알기 위해 다음 코드를 추가합니다.

```java
assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
```

여기서도 테스트 케이스마다 추가로 메모리를 비우기 위해 다음 코드를 작성합니다.

```java
@AfterEach
    public void afterEach(){
        memberRepository.clearStore();
    }
```

### 주의사항

테스트의 Repository는 실제 **MemoryMemberRepository**클래스가 아닌 새로 만든 **MemoryMemberRepository**입니다. 둘은 서로 다른 인스턴스이고 이는 DB를 따로 쓰는 것과 마찬가지입니다.<br>
따라서 테스트와 실제 코드 모두 동일한 **MemoryMemberRepository**를 사용하기 위해서는 **test**패키지 **MemberService**의 코드를 다음과 같이 수정합니다.

#### MemberService 클래스

![image](https://user-images.githubusercontent.com/79408217/157030016-27faab85-9276-4b34-a50b-bf7f18aa9d40.png)

**Alt + Insert > constructor > public MemberService** ... 선택하여 직접 생성하지 않고 외부에서 넣어주는 방식으로 생성합니다.

**MemberService** 입장에서는 **memberRepository**를 직접 new하지 않고 외부에서 넣어준다고 생각할 수 있습니다. 위와 같은 과정을 **DI(Dependency Injection)**이라고 합니다.

#### MemberServiceTest 클래스

![image](https://user-images.githubusercontent.com/79408217/157029535-21e0aa9e-1e64-4b7a-9734-5bbb3d578891.png)

위 코드를 다음과 같이 변경합니다.

![image](https://user-images.githubusercontent.com/79408217/157029751-bc0377bc-9708-4d58-99d7-63aa6f43261e.png)

이렇게 하면 같은 **MemomryRepository**를 사용할 수 있습니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
