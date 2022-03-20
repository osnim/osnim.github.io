---
title: 스프링 입문 20 - 7.스프링 DB 접근 기술 (4)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-20 02:33:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 스프링 JdbcTemplate

**JdbcTemplate**은 이전에 만들었던 **순수 Jdbc**와 동일한 환경설정을 하면 됩니다. <br>
스프링 **JdbcTemplate**라이브러리는 **JDBC API**에서 본 connection 같은 반복적인 코드를 대부분 제거해줍니다. 하지만 SQL은 직접 작성해야 합니다.<br>

**Template**이라고 부르는 이유는 디자인 패턴중에 템플릿 메서드 패턴이라는 것이 있는데 그것이 많이 포함되어서 Jdbc**Template**이라고 부릅니다.

## JdbcTemplate 회원 리포지토리 작성

**jdbcTemplate**는 **injection**을 받을 수는 없습니다.<br>
대신 이전에 **datasource injection** 받은 것 처럼 파라미터에 **DataSource dataSource**를 넣어 **jdbcTemplate 생성자**를 생성합니다.

### save() 기능 구현

![image](https://user-images.githubusercontent.com/79408217/159135088-755a0c72-3ca2-41ea-b03a-088c631b8f2e.png)

```java
      SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());
```

쿼리를 짤 필요 없이 바로 위 4줄로 **INSERT**문을 만들 수 있습니다.<br>
**key**를 **executeAndReturnKey**로 받아서 **member**에 넣어줍니다.<br>
위 코드에 대한 자세한 설명은 **jdbcTemplate DOCUMENT**에 잘 나와있습니다.

### 아이디 조회 쿼리 작성

![image](https://user-images.githubusercontent.com/79408217/159135492-28df2513-3abc-49c1-b4b5-0a64fef593ad.png)

이전에 길었던 코드를 **jdbcTemplate** 라이브러리를 사용하여 단 2줄에 해결할 수 있었습니다.

참고로 **memberRowMapper()** 는 다음과 같습니다.

```java
private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
```

**member** 객체를 생성해서 반환합니다.

### 이름 조회 쿼리 작성

```java
@Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(),name);
        return result.stream().findAny();
    }
```

아이디 쿼리 조회에서 id만 name으로 바꿉니다.

### 전체 조회 쿼리 작성

```java
@Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }
```

위 처럼 간단하게 작성할 수 있습니다.

### 환경 설정 변경 (SpringConfig)

![image](https://user-images.githubusercontent.com/79408217/159135364-6275befc-96d1-4228-8abc-89597e57847b.png)

**JdbcTemplateMemberRepository;**를 연결시켜 줍니다.

## 테스트 실행

만약 이전에 만든 테스트를 실행하지 않고 제대로 작동하는지 확인한다면 일일이 회원을 기입하고 DB에 연동되는 것을 확인하는 번거로운 작업을 해야합니다. 하지만 이전에 만든 테스트 케이스가 있기 때문에 **통합 테스트**를 먼저 실행해보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79408217/159135559-bac0898e-de56-49dd-94a8-4874db190040.png)

에러 없이 잘 작동하는 것을 확인할 수 있었습니다.

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
