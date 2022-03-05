---
title: 스프링 입문 8 - 3.스프링 웹 개발 기초 - API
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-05 20:30:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

정적 컨텐츠를 제외하면 크게 MVC 방식에서 view를 찾아서 템플릿 엔진을 통해서 화면을 렌더링 하여 HTML을 웹 브라우저에 넘기는 방식과 API 방식이 있습니다. 오늘은 API 방식을 알아보도록 하겠습니다.

## API

**HelloController** 클래스에 다음과 같은 메소드를 새로 추가합니다.

```java
@Controller
public class HelloController {

    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name){
        return "hello " + name; // hello spring
    }
```

![image](https://user-images.githubusercontent.com/79408217/156881940-a9642a45-d12b-47e0-8610-5da9113d8a67.png)

**@ResponseBody**: HTTP는 start line, header, body 3부분으로 나뉘는데 **@ResponseBody**가 body에 `"hello" + name;`라는 데이터를 직접 넣어주겠다는 의미 입니다.<br>
여기서 HTTP의 body와 HTML <body> 태그를 헷갈리시면 안됩니다.<br>
즉 서버에게 요청한 클라이언트에게 바로 `hello" + string` 이 바로 전달됩니다.<br>
이것은 템플릿 엔진과 달리 변형없이 바로 전달됩니다.<br>

스프링을 실행하고 **<http://localhost:8080/hello-string?name=osnim>**을 브라우저에 입력했고 다음과 같은 결과를 얻을 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/156882357-dc083b78-94cd-44c1-9c1a-abd3b0a05201.png)

위의 사진이 **helloString** 메소드 입니다. 오른쪽 사진은 소스 코드를 나타낸 것으로 API를 통해 클라이언트의 Request를 Response한 결과는 소스코드를 볼 수 없습니다.
(크롬 관리자 도구를 통해서는 확인이 가능했습니다.)

아래 사진은 비교를 위해 넣은 이전 시간 **HelloMvc**메소드의 Response를 나타냅니다.<br> API와 템플릿 엔진을 활용한 MVC 의 차이가 무엇인지 알아보기 위해 넣었습니다.

하지만 API는 위의 예시처럼 화면을 표현하기 보다는 데이터를 보내기 위해 더 많이 사용합니다.

![image](https://user-images.githubusercontent.com/79408217/156882708-a8ff1781-b2c6-4def-b4bf-e2515468bc67.png)

**HelloController** 클래스에 **Hello** 라는 클래스를 만들어 **Hello**라는 객체를 생성하고 Getter, Setter를 만들어 private 객체를 접근할 수 있는 메서드를 생성합니다.<br>
Windows 환경에서는 Alt + Insert 단축키 > Generate 통해 Getter, Setter를 빠르게 만들 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/156882951-d14767b3-7b30-4ca0-82f9-58a9e6b48de8.png)

**helloApi**라는 메소드를 새로 만들고 hello 라는 객체를 새로 생성합니다.<br>
그 다음 `setName()`을 통해서 name 파라미터를 Hello에 넣어줍니다.

![image](https://user-images.githubusercontent.com/79408217/156883126-19cfaffd-949d-43b9-b2aa-b0cc006c36f6.png)

스프링을 실행하면 화면입니다. 이전과 달리 **API**는 **JSON** 형식으로 값이 반환되는 것을 확인할 수 있습니다.<br>
스프링에서 **API**를 반환하면 기본적으로 **JSON** 형식으로 반환을 하게 세팅이 되어있습니다.

### @ResponseBody 사용원리

![image](https://user-images.githubusercontent.com/79408217/156883422-e63c0c27-77b0-402d-a43a-39442feb218a.png)

스프링에서 받은 **HelloController** 안에 **hello-api**가 있어서 확인을 하는데 **@ResponseBody** 어노테이션이 붙어있는 것을 확인하면 **viewResolver**에 보내지 않고
**HttpMessageConverter**가 동작하여 Body에 JSON 형식으로 데이터를 넣어 클라이언트에 보냅니다.

여기서 문자는 **StringConverter**가 작동하고 문자가 아닌 객체인 경우, **JsonConverter**가 작동합니다.

객체를 JSON으로 바꿔주는 유명한 라이브러리는 크게 Google에서 만든 Gson과 Jackson이 있지만 스프링은 Jackson을 기본으로 탑재했습니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
