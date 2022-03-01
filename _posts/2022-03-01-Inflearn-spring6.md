---
title: 스프링 입문 6 - 3.스프링 웹 개발 기초 - 정적 컨텐츠
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-01 15:00:00 +0900
categories: [Inflearn]
tags: [Inflearn, Spring]
---

웹 개발은 크게 3가지 방법으로 나뉩니다.

1. 정적컨텐츠

   서버에서 처리나 실행없이 파일을 웹 브라우저에 바로 표현하는 것

   스프링 부트는 정적 컨텐츠 기능을 자동으로 지원합니다.

2. MVC와 템플릿 엔진

   - 템플릿 엔진: 정적인 HTML을 바로 웹 브라우저로 보내는 것이 아니라 서버에서 프로그래밍으로 처리 및 변형을 통해 MTML을 동적으로 표현하여 웹 브라우저로 보내는 것입니다.
   - MVC: Model, View, Controller를 통해 템플릿 엔진이 동적으로 웹을 표현하는 것으로 템플릿 엔진을 통해 View를 표현합니다.

   요즘은 MVC와 템플릿 엔진으로 개발을 많이 함

3. API
   JSON 데이터 포맷으로 데이터를 전송하는데

## 정적 컨텐츠

스프링 부트 문서에서 정적 컨텐츠에 대한 설명을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/156116132-99cb3d87-4b31-4526-adad-3371a3c5f293.png)

링크: [스프링 부트 문서 - 정적 컨텐츠 설명](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)

스프링 부트는 static 디렉토리를 먼저 확인하고 없으면 /public or /resources or /META-INF/resources의 폴더들을 확인하여 정적 콘텐츠를 서비스합니다.

![image](https://user-images.githubusercontent.com/79408217/156116947-8263f863-a7ff-488b-8994-1068725f4d39.png)

`static` 이라는 디렉토리에 hello-static이라는 html파일을 생성했습니다.

![image](https://user-images.githubusercontent.com/79408217/156117285-708f9de0-bee3-45d1-962e-1c25ffbcc389.png)

스프링 부트를 실행하고 <http://127.0.0.1:8080/hello-static.html>에 접속을 했습니다.

이렇게 정적 파일이 서버의 어떠한 처리나 변형 없이 바로 웹 브라우저에 적용되는 것을 확인할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/156117572-74672e40-b6d3-4f3b-8a91-b96c8ad0feeb.png)

원리는 크게 이해하면 다음과 같습니다.

1. 웹 브라우저에서 http://127.0.0.1:8080/hello-static.html 를 검색합니다.

2. 내장 톰캣 버서에서 웹 브라우저의 요청을 받아 스프링 부트로 http://127.0.0.1:8080/hello-static.html 입력을 넘깁니다.

3. (1) 스프링의 controller 쪽에서 `hello-static`와 관련된 것이 있는지 찾아봅니다. 컨트롤러가 우선순위를 가집니다. 이전 강의에서 `hello` 라는 컨트롤러는 있었지만 `hello-static`은 컨트롤러가 없었습니다.
   (2) `resouces` 디렉토리에 `static/hello-static.html` 이라는 파일을 찾습니다.
   (3) `static/hello-static.html` 있으므로 아무런 변형 없이 웹 브라우저에 반환을 합니다.

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
