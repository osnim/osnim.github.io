---
title: 스프링 입문 16 - 6.회원 관리 예제 - 웹 MVC 개발
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-14 19:45:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

# 회원 웹 기능

이전시간에 memberController를 만들어 의존관계를 설정해놓은 것을 바탕으로 회원관리 예제를 웹 MVC로 개발 해볼 것입니다. memberController를 통해서 회원을 등록하고 조회 하는 것을 만들어 보겠습니다.

## 1. 홈 화면 추가

![image](https://user-images.githubusercontent.com/79408217/158157870-485cb5f3-5f10-441f-bb82-ea4b8c23d9d3.png)

```java
@Controller
public class HomeController {

    @GetMapping("/")
    public String home(){
        return "home";
    }
}
```

**Controller 패키지**에 **HomeController**를 만들고 **@Controller**를 추가합니다.<br>
이를 통해 **<http://127.0.0.1:8080>**을 접속하면 **("/")** 로 인해 가장 먼저 뜨는 화면을 제어하는 코드입니다.

![image](https://user-images.githubusercontent.com/79408217/158158750-d9efc82c-04ac-4e50-b91c-6c4e367a4200.png)

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <div>
        <h1>Hello Spring</h1>
        <p>회원 기능</p>
        <p>
          <a href="/members/new">회원 가입</a>
          <a href="/members">회원 목록</a>
        </p>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

**return "home"**을 통해 **templates**에서 **home.html** 파일을 서비스 합니다.

![image](https://user-images.githubusercontent.com/79408217/158159361-bb5119a3-6245-460b-bf84-04a4a4e17bb6.png)

실행하면 다음과 같습니다.

### 더 깊게 알아보기

Q. 예전에는 **index.html**가 먼저 브라우저에 서비스 되었는데 이번에는 **home.html** 이 먼저 서비스 되었습니다. 이유는 무엇일까요?

![image](https://user-images.githubusercontent.com/79408217/158159924-332dd252-8ad7-4a00-8f28-62576e53e089.png)

A. 스프링 개발 기초 부분의 **정적 컨텐츠** 편에서 다루었던 내용입니다. 웹 브라우저에서 요청이 먼저 들어오면 스프링은 스프링 컨테이너 부터 스캔하여 컨트롤러의 등록 여부를 판단합니다. 등록되어있지 않으면 그 다음 순서로 static파일을 찾습니다.<br>
즉, **home.html**이 컨테이너에 등록되어있어 **index.html** 보다 먼저 서비스 되는 것입니다.

```html
<p>
  <a href="/members/new">회원 가입</a>
  <a href="/members">회원 목록</a>
</p>
```

**home.html**을 자세히 살펴보면 위와 같이 `<a>` 태그로 링크가 달려있는 것을 확인할 수 있습니다.
이를 통해 글씨를 클릭하면 다른 페이지로 이동합니다.

## 2. 등록

**MemberController 클래스**에 2가지 메소드를 추가합니다. 홈 화면에서 클릭시 이동하는 페이지입니다.

**templates** 디렉토리에 **members**디렉토리를 만들고 **createMemberForm.html**파일을 생성하고 코드는 다음과 같습니다.

```html
<!-- createMemberForm.html -->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <form action="/members/new" method="post">
        <div class="form-group">
          <label for="name">이름</label>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="이름을 입력하세요"
          />
        </div>
        <button type="submit">등록</button>
      </form>
    </div>
    <!-- /container -->
  </body>
</html>
```

위 코드를 간략하게 설명드리면,

1. **<form action="/members/new" method="post">**

`<form>` 태그의 action 속성은 form 데이터(form data)를 서버로 보낼 때 해당 데이터가 도착할 URL을 명시합니다. 그래서 **<http://localhost:8080/members/new>**로 데이터를 보냅니다.<br>

`<form>` 태그의 method 속성은 폼을 서버에 전송할 http 메소드를 지정하는데 크게 GET과 POST 방식이 있습니다. POST는 클라이언트에서 서버로 데이터를 보낼때, GET은 클라이언트가 서버에서 데이터를 가져올 때 사용합니다.<br>

2. **name="name"**

여기서 name이 서버로 넘어갈 때 **key**역할을 합니다.

3. **placeholder**

아무것도 입력이 안되었을 때 보여지는 글씨입니다.

![image](https://user-images.githubusercontent.com/79408217/158166544-2bf6210f-d169-4233-80d3-2e1485dc7950.png)

스프링을 실행시킨 화면입니다. 이름을 입력하면 **{이름: osnim}**이 **키-값** 쌍으로 전달됩니다.

![image](https://user-images.githubusercontent.com/79408217/158168043-1f066b60-e1bd-4134-a4e9-9916aa71dec9.png)

회원 등록하는 컨트롤입니다. **controller 패키지**에 **MemberForm 클래스**를 만들고 **MemberForm** 안에 변수 **name**과 **getter, setter** 를 생성합니다. <br>
**MemberForm 클래스** **name**은 **createMemberForm.html**의 **name**과 매칭이 됩니다.<br>

![image](https://user-images.githubusercontent.com/79408217/158168247-b8b933cc-5d53-481a-99e3-41f2eb0a18ec.png)

**MemberController** 클래스에 **create 메서드**를 추가합니다. **form** 으로 새로운 회원 정보를 받아 객체를 생성하고 **이름**을 **set**하고 **redirect**를 이용해서 **홈 화면**으로 보냅니다.
지금은 이름을 기입하고 **등록**을 눌러도 회원 목록을 볼 수 없지만 나중에 회원 목록 기능을 추가하면 확인이 가능합니다.

#### 복습 - MemberService의 join 메서드

![image](https://user-images.githubusercontent.com/79408217/158168716-62d6420e-ec60-4596-addf-0e4cd6adff9c.png)

그 후 이전에 만든 **join 메서드**로 이름 중복 확인 후 **save** 합니다.

## 회원 가입 동작 원리

![image](https://user-images.githubusercontent.com/79408217/158171148-29ce8a41-d577-4c85-95e4-246383bd578e.png)

**method**가 **post** 방식이라서 데이터를 서버로 전송할 때 사용합니다. 따라서 **"members/new"**라는 URL은 같지만 **GET, POST**에 따라 다르게 맵핑할 수 있습니다.

참고로 **Get** 방식은 URL을 직접 쳐서 들어가는 방식으로 조회할 때 주로 사용합니다.

![image](https://user-images.githubusercontent.com/79408217/158180072-5333295a-d96d-4edf-a089-08effdb27a2f.png)

먼저 **createMemberForm.html**의 **name**이 **MemberForm**에 들어갈 때 스프링이 알아서 **name**이라는 **key**를 확인하고 **@PostMapping**어노테이션을 확인하고 **MemberForm**을 받아 저장합니다.

## 3. 조회

회원 관리 기능중에서 조회 기능을 구현할 것입니다. **MemberController 클래스**에 다음을 추가합니다.

```java
@GetMapping("/members")
    public String list(Model model){
       List<Member> members = memberService.findMembers();
       model.addAttribute("members", members);
       return "members/memberList";
    }
```

이전에 했던 것과 비슷하게 **model.addAttribute**를 이용해서 작성합니다.

```html
<!--memberList.html-->
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <div class="container">
      <div>
        <table>
          <thead>
            <tr>
              <th>#</th>
              <th>이름</th>
            </tr>
          </thead>
          <tbody>
            <tr th:each="member : ${members}">
              <td th:text="${member.id}"></td>
              <td th:text="${member.name}"></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- /container -->
  </body>
</html>
```

회원 목록 버튼 클릭시 이동되는 페이지의 html 코드입니다. 여기서는 **thymeleaf**가 동작합니다. 위 코드를 타임리프가 렌더링해서 웹 브라우저에 서비스 합니다.<br>
**${members}** 에서 **$** 는 **model** 안에 있는 값을 꺼내는 역할을 합니다.
**th**는 **thymeleaf** 문법을 나타내며, **each**는 **loop**을 의미합니다. **${members}"**에서 객체를 꺼내서 **member**에 담고 그 **member**에 속한 **id**와 **name**을 출력합니다.

![image](https://user-images.githubusercontent.com/79408217/158200989-c3042594-df20-41c5-9338-4412f4baf170.png)

스프링을 실행하고 이름을 기입하여 회원 목록을 확인하니 저장된 이름들이 출력되는 것을 확인할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/
158201390-9af95f5b-aada-430e-9e9d-7b38d8e29217.png)

페이지 소스 코드를 확인하니 **memberList.html**와 달리 **thymeleaf**로 인해 렌더링 된 소스코드가 출력되는 것을 확인할 수 있었습니다.

데이터가 DB가 아닌 메모리에 있기 때문에 스프링을 재시작하면 회원 목록들이 모두 사라지게 됩니다.

## Tip

- 변수를 클릭하고 **Shift + F6** 누르면 단락에 있는 이름이 같은 변수들을 한번에 바꿀 수 있습니다.
- **Alt +Enter** 를 누르면 바로 Local 가져올 수 있습니다.
- 인텔리제이 주석
  줄 단위 : ctrl + /
  블록단위 : ctrl + shift + /
- getter, setter 단축키 : Alt + Insert

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 들으며 공부한 것을 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
