---
title: 스프링 입문 18 - 7.스프링 DB 접근 기술 (2)
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-19 20:24:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 순수 JDBC

이번 시간은 애플리케이션과 DB를 연동해서 DB에 데이터를 실제로 저장해보겠습니다. 요즘은 이런 방식으로 진행하지 않습니다.

## 환경 설정

### 1. **build.gradle** 파일에 라이브러리를 추가합니다.

    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    runtimeOnly 'com.h2database:h2'

자바는 기본적으로 DB와 연동되기 위해서는 **JDBC**가 필요합니다.<br>
**H2 데이터베이스**와 연동되기 위해서는 **h2 클라이언트**가 필요합니다.

### 2. DB 경로 설정

**src > main > resources > application.properties** 파일에 다음을 추가합니다.

    spring.datasource.url=jdbc:h2:tcp://localhost/~/test
    spring.datasource.driver-class-name=org.h2.Driver
    spring.datasource.username=sa

만약 빨간불이 뜰 경우 **import**가 안된 것으로, 옆에 코끼리를 눌러주면 **최신화**를 자동으로 해줍니다.<br>

#### 주의사항

스프링부트 2.4 부터는 **spring.datasource.username=sa**를 꼭 추가해야 합니다.<br>
그렇지 않으면 **Wrong user name or password**가 발생합니다. 또한 **sa** 뒤에 **공백**이 있으면 에러가 발생하므로 공백은 모두 없애야 합니다.

## API를 통한 개발

이제는 **JDBC, API**를 통한 개발을 해보도록 하겠습니다.<br>
지금까지 회원을 저장하는 역할이나 기능은 **MemberRepository**가 했습니다. 그리고 저장은 **MemoryMemberRepository**에 저장했습니다. <br>
하지만 지금은 DB랑 연동해서 **JDBC**로 저장을 진행할 것입니다.

먼저 **repository**에 **JdbcMemberRepository** 클래스를 생성합니다.

![image](https://user-images.githubusercontent.com/79408217/159119838-28ec5d14-f443-4eb1-8669-f5c1a8d7486d.png)

![image](https://user-images.githubusercontent.com/79408217/159119886-26858369-59d8-480c-b197-2089e910c868.png)

그 후 **implements MemberRepository**를 추가하고 빨간줄은 우클릭을 하여<br>
**Show Context Actions > Implement methods > 4개 모두 선택 > OK** 선택합니다.

![image](https://user-images.githubusercontent.com/79408217/159178614-527aef94-e4a3-46e2-8451-1b5e82a8cd4f.png)

이제 하나씩 추가합니다.

**private final DataSource dataSource;** 를 추가합니다.

```java
public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }
```

스프링을 통해서 **dataSource** 을 주입 받습니다.

### 1. save() 메소드

![image](https://user-images.githubusercontent.com/79408217/159126770-cb796d2e-3738-4ace-a1ac-f0472cb05a3f.png)

```java
String sql = "insert into member(name) values(?)";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;

        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
            pstmt.setString(1, member.getName());
            pstmt.executeUpdate();
            rs = pstmt.getGeneratedKeys();
            if (rs.next()) {
                member.setId(rs.getLong(1));
            } else {
                throw new SQLException("id 조회 실패");
            }
            return member;
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
```

- **ResultSet rs** : 결과를 만드는 것입니다.
- **getConnection()** : DB 와의 connection을 가져옵니다.
- **RETURN_GENERATED_KEYS** : DB에 insert를 하면 1, 2 같은 연속적인 순서를 얻을 수 있게 해줍니다.
- **setString(1, member.getName());**: **1**은 **values(?)** 의 **?**에 매칭이 됩니다. 그 후 **member.getName()** 으로 값을 넣습니다.
- **pstmt.executeUpdate();** : DB에 실제 쿼리를 전달합니다.
- **rs = pstmt.getGeneratedKeys()** : DB가 방금 생성한 키를 반환합니다. **RETURN_GENERATED_KEYS**와 매칭되어 사용할 수 있습니다.
- **rs.next()** : rs(resultSet)이 순서 값을 가지고 있는데 **next**에 값이 있으면 값을 꺼내줍니다.
- **close** : 복잡하다 정도만 알고 있으면 됩니다. 요즘은 이렇게까지 디테일하게 구현을 안해도 된다고 합니다.

### 2. findById() 메소드

![image](https://user-images.githubusercontent.com/79408217/159127184-68aaa06b-21c2-4afa-ae57-40fc0372ef60.png)

```java
@Override
    public Optional<Member> findById(Long id) {
        String sql = "select * from member where id = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setLong(1, id);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            } else {
                return Optional.empty();
            }
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
```

**findById()** 조회 기능입니다. 크게 특별한 건 없고 **rs.next()** 가 값이 있으면 값을 member에 넣어줍니다.

### 3. findByName() 메소드

![image](https://user-images.githubusercontent.com/79408217/159127354-f30b07f2-56c8-4639-8216-05ed36d2c960.png)

```java
@Override
    public Optional<Member> findByName(String name) {
        String sql = "select * from member where name = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, name);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            }
            return Optional.empty();
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }
```

### 4. findAll() 메소드

![image](https://user-images.githubusercontent.com/79408217/159127374-38c158d7-8d73-4ce7-94d0-ed34664236c5.png)

```java
@Override
    public Optional<Member> findByName(String name) {
        String sql = "select * from member where name = ?";
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        try {
            conn = getConnection();
            pstmt = conn.prepareStatement(sql);
            pstmt.setString(1, name);
            rs = pstmt.executeQuery();
            if(rs.next()) {
                Member member = new Member();
                member.setId(rs.getLong("id"));
                member.setName(rs.getString("name"));
                return Optional.of(member);
            }
            return Optional.empty();
        } catch (Exception e) {
            throw new IllegalStateException(e);
        } finally {
            close(conn, pstmt, rs);
        }
    }

```

전제조회 기능을 하는 findAll() 메소드입니다. List로 member를 members에 담아서 반환합니다.

### 5. 기타 메소드

![image](https://user-images.githubusercontent.com/79408217/159127467-bf1db9e0-1d15-4fa8-9fe7-a5f3e8cd0bd1.png)

순수 JDBC에 필요한 기타 메소드입니다. **DatasourceUtils**를 이용해서 Connection 메소드와 close메소드를 구현합니다. 하지만 기타 메소드 뿐만 아니라 위 코드는 쓸 일이 거의 없기 때문에 참고로 알아두시면 좋습니다.

## Configuration 변경

위 코드만으로는 실행이 불가능합니다.

![image](https://user-images.githubusercontent.com/79408217/159127622-02bd80d1-8f0b-4669-835a-8be8fec0a997.png)

추가로 **service** 패키지의 **SpringConfig** 클래스를 변경합니다.<br>
**DataSource**는 데이터베이스 커넥션을 획득할 때 사용하는 객체입니다.<br>
스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어줍니다. 그래서 DI를 받을 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/159127828-1688e1b9-ea38-45fa-b619-26e30813084a.png)

**memberRepository** 메소드의 반환 값을 **new JdbcMemberRepository()**로 변경합니다.<br>
**dataSource**를 **DataSource형**으로 선언하고 **SpringConfig(DataSource dataSource)** 메소드에 **dataSource 의존성**을 추가합니다.<br>
스프링이 DB까지 관리할 수 있게 **@Autowired**를 추가하여 스프링 빈으로 만들어 줍니다.<br>
여기서 중요한 점은 main안에 있는 다른 코드는 수정하지 않고 **SpringConfig 파일** 하나만 수정하였습니다.

## 스프링 실행 결과

위 설정들을 모두 마치고 스프링을 실행했습니다. 주의할 점은 H2 DB가 실행되는 상태에서 스프링을 실행해야 합니다.

![image](https://user-images.githubusercontent.com/79408217/159128487-f84579a7-eaf8-4fdf-951f-7bee219bfbec.png)

먼저 H2 DB의 상태를 바로 웹 **회원 목록**에서 확인할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/159128596-89c069f2-c041-4e2d-94aa-e7a18481404c.png)

그리고 웹에서 `jpa` 라는 이름을 추가하여 회원가입 시킨 후, 회원 목록과, H2 콘솔에서 확인한 결과입니다. 모두 잘 작동하는 것을 확인할 수 있었습니다.

데이터를 DB에 저장하였기 때문에 스프링 서버를 다시 실행해도 데이터가 안전하게 저장된 것을 확인할 수 있습니다.

## 정리

왜 스프링을 사용하는지 이번에 자세하게 확인할 수 있었습니다.<br>

![image](https://user-images.githubusercontent.com/79408217/159129219-b108233a-b4ac-4b73-bed9-be2af8dd25c8.png)

![image](https://user-images.githubusercontent.com/79408217/159129207-98122d34-01d6-45c6-9de9-747757293d2f.png)

객체지향적 설계의 장점 중 하나인 **다형성**이 이번 실습에 잘 드러나 있는데 인터페이스인 **MemberRepository**를 사이에 두고 구현체인 **MemoryMemberRepository**와 **JdbcMemberRepository**를 쉽게 갈아 탈 수 있습니다. <br>
스프링은 이 장점을 매우 편리하게 사용할 수 있도록 스프링 컨테이너가 지원을 해줍니다.

과거의 경우 의존성을 모두 일일이 수정해야 했습니다. 가령 **MemberService**가 **MemoryMemberRepository**를 의존하는 것에서 **JdbcMemberRepository**를 의존하는 것으로 코드를 수정해야하는 번거로움이 있었습니다.<br>

이번 실습의 경우, **MemberService**만 있어서 간단하게 실행해볼 수 있었지만 실제 웹이나 앱을 만들 경우 **OrderService**, **QnAService**, **AdService** 등 여러가지 기능이 생길 경우 모두 수정해야 합니다.<br>

하지만 지금은 스프링 부트의 존재로 애플리케이션를 조립하는 어셈블리(**SpringConfig**)부분만 수정하면 되어서 매우 간편하게 수정 및 개발을 진행할 수 있습니다.

- **개방 폐쇠의 원칙(OCP, Open-Closed Principle)**
  **확장에는 열려있고, 수정, 변경에는 닫혀있다** 라는 말로, 구현체 등을 여러 개 붙이는 **확장**에는 열려있고 그 구현체 내부를 **수정, 변경**은 닫혀있다는 스프링의 장점 중 하나입니다.

- 스프링의 **DI (Dependencies Injection)**을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현
  클래스를 변경할 수 있다. 즉 저는 **Bean**으로 등록만 했고 나머지는 스프링이 알아서 의존성을 주입해주어 의존관계를 알아서 설정해주었습니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /
- getter, setter 단축키 : Alt + Insert

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
