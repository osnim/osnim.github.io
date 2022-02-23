---
title: 스프링 입문 - 4.View 환경설정
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-02-23 14:00:00 +0900
categories: [Inflearn]
tags: [Inflearn, Spring]
---

## Error 페이지를 Welcome Page 만들기

![image](https://user-images.githubusercontent.com/79408217/155263932-e022b682-8603-4cd6-a717-a64c0fb455bf.png)

src > main > java > resources > static 폴더에서 우클릭을 눌러 New > file을 클릭하고 새로운 file 하나를 생성합니다.

파일명은 index.html을 입력합니다.

그리고 다음 html 코드를 입력합니다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
    Hello
    <a href="/hello">hello</a>
  </body>
</html>
```

main 을 실행하고 브라우저에서 <http://localhost:8080/> 를 입력하면 다음과 같은 페이지를 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155264206-6d369434-45a1-4911-9a1c-a000734c8a11.png)

이렇게 welcome 페이지를 쉽게 제작할 수 있습니다.

스프링 부트는 스프링 생태계를 감싸서 편리하게 개발하는데 도움을 줍니다.

스프링은 java enterprise web 애플리케이션 개발과 관련된 생태계를 모두 제공하므로 매우 광범위합니다. 그래서 모두 사용하지 못하고 필요한 것을 적절하게 찾아 사용하는 것이 중요합니다.

welcome 페이지 제작은 다음과 같은 순서로 진행하면 수월합니다.

![image](https://user-images.githubusercontent.com/79408217/155264690-4813bfed-7e43-4294-880c-250f2a91e2af.png)

먼저 [**spring.io**](https://spring.io/)를 접속하여 상단 바에서 Project Spring Boot 메뉴를 선택합니다.

![image](https://user-images.githubusercontent.com/79408217/155271158-9a1451d4-9e4a-4e6e-b170-a9dbe1a547ce.png)

그 후 LEARN 의 2.6.3 버전(자산의 버전)의 Reference Doc. 를 선택합니다.

![image](https://user-images.githubusercontent.com/79408217/155271541-4d034d1c-c16e-40e6-bb57-2f510d1458bd.png)

![image](https://user-images.githubusercontent.com/79408217/155271623-e4c358aa-3fa4-4b5e-b525-bc5e62b38a4a.png)

Reference Doc페이지 왼쪽의 Web 메뉴를 선택하고 Cntl + F 눌러 Welcome 을 입력합니다.
스프링 부트는 가장 먼저 index.html을 찾고 없으면 index 템플릿을 찾는 방식으로 작동을 합니다.

## Thymeleaf 템플릿 엔진

index.html은 정적페이지로 아무런 동작을 하지 않고 페이지를 띄우기만 합니다. 하지만 Thymeleaf 템플릿 엔진을 사용하면 welcome 페이지를 변경할 수 있습니다.

- thymeleaf 공식 사이트: <https://www.thymeleaf.org/>
- 스프링 공식 튜토리얼: <https://spring.io/guides/gs/serving-web-content/>
- 스프링부트 메뉴얼: <https://docs.spring.io/spring-boot/docs/2.3.6.RELEASE/reference/htmlsingle/>

![image](https://user-images.githubusercontent.com/79408217/155272258-f929d389-48b7-46ee-8ac6-2d116c9fffc6.png)

스프링부트 메뉴얼 사이트에 접속해서 Template Engines 를 입력하면 제공되는 4가지 엔진을 확인할 수 있습니다.

- FreeMarker
- Groovy
- Thymeleaf
- Mustache

이 중에서 저는 Thymeleaf를 선택했습니다.

![image](https://user-images.githubusercontent.com/79408217/155272547-228dc76e-fe95-4d47-ba21-d9552b25c0d9.png)

웹 애플리케이션의 진입점이 controler이기 때문에 IntelliJ로 돌아와서 controller를 만들어야합니다.

위치는 Hello.Hellospring 패키지에 새로운 **controller** 패키지를 만듭니다.

![image](https://user-images.githubusercontent.com/79408217/155272865-24f53253-72bc-4edd-bec6-c177c2d994cb.png)

![image](https://user-images.githubusercontent.com/79408217/155273011-deae615d-0fa4-409b-893a-86e1ab84b7f7.png)

이후 **controller** 패키지에서 **HelloController** 라는 새로운 java class를 생성합니다.

![image](https://user-images.githubusercontent.com/79408217/155273165-4061de79-63ec-4d4e-b940-f50e9ca91f3b.png)
![image](https://user-images.githubusercontent.com/79408217/155273191-a1e6b404-d72a-46a7-83e3-684175b6c108.png)

그 후 controller위에 @Controller 어노테이션을 추가합니다.

![image](https://user-images.githubusercontent.com/79408217/155273943-538d3183-fa92-442a-881b-268833dc5578.png)

그리고 위와 같은 코드를 작성합니다.

이 코드는 웹 애플리케이션에서 `/hello` 라고 들어오면

```java
public String hello(Model model){
    model.addAttribute("data", "hello!");
    return "hello";
    }
```

이 메소드를 호출합니다.

여기서 model은 MVC에서의 model을 의미합니다.

![image](https://user-images.githubusercontent.com/79408217/155274082-1ba719a4-5c81-4681-bd64-98808cf9d622.png)
그 다음 template 폴더에 hello.html 이라는 새로운 파일을 생성하고 다음과 같은 코드를 넣습니다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  </head>
  <body>
    <p th:text="'안녕하세요. ' + ${data}">안녕하세요. 손님</p>
  </body>
</html>
```

th는 thymeleaf 를 의미하며 th="http://www.thymeleaf.org" 를 통해 xmlns 스키마로 미리 선언되어있음을 확인할 수 있습니다.

위 코드의 `${data}"` 는 HelloController 자바 클래스 안의 hello 메소드의 `hello!`로 치환이 되는데 이는 모델에서 key값을 의미 합니다.

다시 main 메소드를 실행하면 <http://localhost:8080/>에서 다음과 같은 결과를 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155274632-d1f4b150-eb66-4f82-9fed-2e354e50d525.png)

## 동작 환경 그림

![image](https://user-images.githubusercontent.com/79408217/155274759-d859b0f3-a1a1-4bcc-93a3-32ecd0b3fd06.png)

먼저 웹 브라우저에서 <http://localhost:8080/> 를 tomcat 내장서버에 보내면 tomcat이 스프링 컨테이너에게 질문을 합니다.

그러면 스트링 컨테이너는 `HelloController`안에 있는 `GetMapping`을 통해(GET, POST의 그 GET 맞습니다.) `hello`를 확인하여 `public String hello(Model model)` 메소드를 실행시킵니다.

그 후 스프링이 모델을 넣어 같이 보내줍니다. 그 모델안에 key는 `data`이고 값은 `hello!`를 넣어 `hello` 를 리턴합니다.

이는 resources > templates 폴더에 있는 hello라는 파일을 찾아서 이것으로 렌더링 하라(이 화면을 실행시켜라)는 return 명령입니다.

컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(viewResolver)가 화면을 찾아서 처리합니다.
즉, 스프링 부트 템플릿엔진 기본 viewName 매핑하는데 이는

**resources:templates/ +{ViewName}+ .html**

이런 구조로 되어있고 ViewName은 hello로 바뀌게 되어 hello.html이 열리게 됩니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
