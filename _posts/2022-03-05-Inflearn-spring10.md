---
title: 스프링 입문 10 - 4.회원 관리 예제 - 백엔드 개발 (2)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-05 22:26:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 회원 도메인과 리포지토리 만들기

## 회원 객체 생성

![image](https://user-images.githubusercontent.com/79408217/156885477-9d55f41f-14d3-449f-a533-c75aaf42fd96.png)

**Hello.Hellospring** 패키지에 **domain**이라는 패키지를 생성합니다. <br>
**Member**라는 클래스를 생성하고 **id**, **name** 이라는 변수를 선언합니다.

여기서 id는 사용자가 입력한 id가 아닌 시스템이 회원을 구분하지 위한 고유의 id 값입니다.

그 후 getter, setter 객체를 생성합니다.

### 리포지토리 생성

**Hello.Hellospring** 패키지에 **MemberRepository**를 클래스가 아닌 인터페이스로 생성합니다.

![image](https://user-images.githubusercontent.com/79408217/156885831-2062992e-7040-4478-aa24-cf871d8ae349.png)

**import Hello.Hellospring.domain.Member;**를 추가하여 Member에서 빨간색을 없애줍니다.
빨간줄을 없애는 방법은 Member 우클릭 > Show Context Action > import class > Hello.Hellospring.domain.Member 선택

![image](https://user-images.githubusercontent.com/79408217/156885995-e735bee8-cb97-457a-9504-30c5bd25991c.png)

Optional을 추가하고 빨간줄을 없애는 방법입니다. Optional은 Java 8 부터 있는 기능으로 `findById`를 가져올 때, NULL 값일 수 있는데 이를 처리를 할 때, Optional을 감싸서 처리를 합니다.

```java
package Hello.Hellospring.repository;
import Hello.Hellospring.domain.Member;
import java.util.List;
import java.util.Optional;

public interface MemberRepository {
    Member save(Member member); //회원 저장
    Optional<Member> findById(Long id); //회원을 id로 찾기
    Optional<Member> findByName(String Name); //회원을 이름으로 찾기
    List<Member> findAll(); //저장된 모든 회원 출력
}
```

**MemberRepository** 코드에 리포지토리의 save, findById, findByName, findAll이라는 4가지 기능을 만들었습니다.

### 구현체 생성

**Repository** 패키지에 **MemoryMemberRepository**라는 클래스를 생성하고 **MemberRepository**를 **implements**합니다.<br>

![image](https://user-images.githubusercontent.com/79408217/156886542-b693c6f0-792f-4013-888f-f3aec5304f7e.png)

빨간줄이 뜨는 건 위와 같이 해결합니다.

![image](https://user-images.githubusercontent.com/79408217/156886568-4832ec53-8b8c-49f3-8ed1-db24d69db6d4.png)

값을 저장할 공간을 구현하기 위해 Map을 사용하여 키-값 쌍 List를 만듭니다. Map 빨간줄을 없애기 위해 이전처럼 Map을 Option에서 import합니다.<br>
sequence: 키값인 id를 자동으로 생성해주는 변수입니다.

**MemoryMemberRepository** 클래스 내부의 구체적은 메소드들의 내용은 다음과 같습니다.

1. save()

```java
@Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }
```

2. findById()

```java
    @Override
    public Optional<Member> findById(Long id) {
        return Optional.ofNullable(store.get(id));
    }
```

**Optional**은 만약 get함수가 NULL값을 가져와도 클라이언트가 에러가 발생하지 않고 처리가 가능하다.

3. findById()

```java
@Override
    public Optional<Member> findByName(String name) {
        return store.values().stream()
                .filter(member -> member.getName().equals(name))
                .findAny();
    }
```

stream(): 루프를 돌리는 함수입니다.
filter(): 파라미터의 name과 member의 name과 같은지 비교하는 함수입니다.
findAny(): `name`과 같은 이름을 하나라도 찾는 함수입니다.

4. findAll()

```java
 @Override
    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }
```

`private static Map<Long, Member> store = new HashMap<>();`를 보면 Map형식이지만
findAll()의 반환은 List입니다.
`store.values()`는 `Map<Long, Member>`에서의 Member들 가리킵니다.

전체 코드는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156887343-9b03e127-93a6-46c0-95e8-c39c48896104.png)

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
