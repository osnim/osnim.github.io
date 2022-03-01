---
title: 스프링 입문 7 - 3.스프링 웹 개발 기초 - MVC와 템플릿 엔진
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-01 15:00:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

## MVC

과거에는 view와 controller 를 구분하지 않았습니다. 그래서 view에 controller 기능을 넣어서 JSP를 가지고 개발했습니다. 이 방식을 모델 1 방식이라고 부릅니다.

지금은 MVC 패턴 방식으로 개발을 많이 합니다. 개발을 할 때, 기능에 따라 분리해서 개발해야 효율이 늘어나고 유지보수가 수월해집니다.

M : Model - 화면에서 필요한 요소를 모델에 담아 View 에 넘겨주는 역할

V : View - 보여지는 것에 초점을 맞추어 개발

C : Controller - 비즈니스 로직과 관련되어 있거나 서버와 관련된 처리하는 데에 집중하여 개발

## 새로운 controller 생성

### HelloController 클래스

![image](https://user-images.githubusercontent.com/79408217/156136901-3c4ecf5a-9059-48e8-98af-0f30bb1bd61f.png)

HelloController 클래스에 `hello-mvc` controller 생성합니다.

`hello-mvc`는 파라미터를 받을 수 있게 `@RequestParam("name")` 과 `String name`, `Model model`를 입력합니다.

이전 `hello controller`는 `hello!` 라는 값으로 고정이 되어있었다면, name은 웹 사이트에서 입력받은 값을 그대로 model로 전달합니다.

`model.addAttribute()`를 사용해서 파라미터에서 넘어온 name을 model로 넘겨줍니다.
`attribureName "name"`이 key 이고 사용자가 입력한 name이 전달됩니다.

```java
return "hello-template"
```

을 통해 `hello-template.html`로 이동합니다.

### hello-template.html

![image](https://user-images.githubusercontent.com/79408217/156136760-d4f892c3-c0e0-4f17-a967-60512ab947e6.png)

`tempaltes 디렉토리`에 `hello-template.html` 을 생성하고 다음과 같은 코드를 입력합니다.

```html
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <p th:text="'hello ' + ${name}">hello! empty</p>
  </body>
</html>
```

thymeleaf 를 통해 서버없이 html을 바로 웹브라우저에서 열 수 있습니다.

**hello! empty**는 `hello-template.html` 파일을 템플릿 동작이 없을 때 웹 브라우저에서 출력되지만 템플릿이 동작하여 `hello-template.html`을 열면 **hello! empty** -> `'hello ' + ${name}` 로 치환되어 출력됩니다.

`$`는 `model` 에서 `key` 값이 `name`인 `value`로 치환하여 출력한다는 의미입니다.

즉 **hello! empty**은 서버 없이 html 파일을 볼 때, 입력하는 코드 입니다.

스프링을 실행하고 <http://127.0.0.1:8080/hello-mvc>에서 다음 화면을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/156138525-3a09670a-4143-4e8f-8d72-858d422d9a9c.png)

에러 화면이 나왔지만 이것이 **정상**입니다. **에러**를 고치기 위해서는 [**에러 수정**](#에러-수정) 를 확인해주세요

정상적으로 웹 화면에 html을 출력하기 위해서는 `http://127.0.0.1:8080/hello-mvc` 뒤에 `?name=spring!!`을 붙여줍니다.

이는 HTTP에서 GET 방식으로 파라미터를 넘겨주는 방식으로 다음과 같이 에러 페이지가 나오지 않고 정상적으로 웹 화면이 나오는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/156143429-01a5c24b-cb09-4bf3-831e-490865054e9b.png)

스프링을 시작하고 name 파라미터가 어떻게 `hello-template.html`에 넘어가는지를 순서를 정리한 화면입니다.

여기서 model이 View에 필요한 요소들을 전달하는 것을 확인할 수 있었습니다.

오늘 강의를 요약하면 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156144600-1546d845-e85b-4fef-9e57-91c5b7194868.png)

웹 브라우저에서 `http://127.0.0.1:8080/hello-mvc`를 스프링 부트로 넘기면 스프링 부트를 띄울 때 같이 띄우는 내장 톰캣 서버를 먼저 거칩니다.

내장 톰캣 서버는 `http://127.0.0.1:8080/hello-mvc`를 스프링 부트로 보냅니다.

`hello-mvc` 가 `helloController 메소드`에 매핑이 되어 있으므로
`public String HelloMvc HelloMvc` 라는 메소드를 호출하고 model로 key=name, value = spring 데이터를 `hello-template`와 같이 리턴합니다. (ViewResolver에 전달합니다.)

`ViewResolver`는 `templates` 디렉토리에서 `hello-template.html`을 찾아서 `thymeleaf 템플릿 엔진`에 `hello-template.html`을 넘깁니다.

`thymeleaf 템플릿 엔진`이 렌더링(변환 및 처리)을 해서 HTML을 웹 브라우저에 반환합니다.

![image](https://user-images.githubusercontent.com/79408217/156146274-b455177a-b999-4638-a98d-418e9701f6ad.png)

변환되어 웹 브라우저에 반환된 HTML은 원본 `hello-template.html` 코드와 다른것을 확인할 수 있습니다.

이전 강의에서는

### 에러 수정

에러를 고치기 위해 IntelliJ 로그화면에 찍힌 WARN 14796 코드를 확인해보았습니다.

WARN 14796 --- [nio-8080-exec-2] .w.s.m.s.DefaultHandlerExceptionResolver : Resolved [org.springframework.web.bind.MissingServletRequestParameterException: Required request parameter 'name' for method parameter type String is not present]

`parameter 'name'` 가 없다는 에러입니다.

![image](https://user-images.githubusercontent.com/79408217/156138994-81bf9b8f-36a8-4283-be50-ce31cf92fcdd.png)

`HelloMvc` 메소드의 `name` 파라미터 부분을 다음과 같이 고쳐줍니다.

![image](https://user-images.githubusercontent.com/79408217/156139738-a1724e53-cc49-4adb-86d5-42c53e85925e.png)

커서를 놓고 `Ctrl + P` 를 누르면 옵션을 선택할 수 있습니다. required 를 입력하고 `Ctrl + P `를 눌러 `false` 를 넣어줍니다.

cf) `true`는 반드시 입력값이 있어야 한다는 의미입니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
