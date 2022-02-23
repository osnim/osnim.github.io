---
title: 스프링 입문 - 2.프로젝트 생성
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-02-22 13:50:00 +0900
categories: [Inflearn]
tags: [Inflearn, Spring]
---

## 프로젝트 생성

    사전 준비
    - java 11 설치
    - IDE: IntelliJ 또는 Eclipse 설치

    스프링 부트 스타터 사이트로 이동해서 스프링 프로젝트 생성합니다
    예전에는 스프링 프로젝트를 처음부터 다 만들었다면 요즘은 대부분 스프링 부트로 스프링 프로젝트를 만듭니다.
    - URL : <https://start.spring.io>

## 실제 프로젝트 생성

![image](https://user-images.githubusercontent.com/79408217/155076717-e4a1df22-8c74-4ce2-a7bd-d95e3513f90f.png)

- Project
  필요한 라이브러리를 가져오고 빌드하는 라이프 사이클까지 모두 관리해주는 툴 과거는 Maven으로 만들었지만 요즘은 Gradle로 넘어오는 추세입니다.

- Language <br>
  Java

- Spring Boot<br>
  버전을 선택해야하는데 SNAPSHOT, M1은 정식 릴리즈된 버전이 아니므로 선택하지말고 정식 버전인 2.6.X을 선택해야 합니다.

- Group<br>
  보통 기업의 도메인이나 이름을 넣지만 입문자는 크게 상관이 없으므로 Hello 를 넣습니다.

- Artfact<br>
  빌드된 후 나오는 결과물로 프로젝트명이라고 보시면 됩니다.

- Name<br>
  설정 값을 그대로 유지합니다.

- Description<br>
  설정 값을 그대로 유지합니다.

- Pakage name<br>
  설정 값을 그대로 유지합니다.

![image](https://user-images.githubusercontent.com/79408217/155077332-2a16993d-d2e6-4d6e-81ad-ec6ec4f4b1c8.png)

- Dependencies<br>
  스프링 부트를 시작할 때 어떤 라이브러리를 가져와 쓸 것인지 결정합니다.
  저희는 웹 프로젝트를 생성할 것이기 때문에 **spring web**을 입력하고 선택합니다.
  추가로 웹 브라우저에서는 HTML이 보이듯이 스프링 부트에서 HTML을 만들어주는 **Thymeleaf** 템플릿 엔진이 필요합니다.

모든 설정을 마치고 **GENERATE** 버튼을 클릭하여 다운로드를 받습니다.

다운로드 받은 압축 파일의 압축을 풀고 IntelliJ 같은 IDE로 build.gradle 파일을 엽니다. 이후 90M 정도의 외부라이브러리를 다운 받기 때문에 완료가 될 때 까지 기다려줍니다.

## 스프링 부트 파일 분석

- .idea<br>
  IntelliJ 가 사용하는 설정 파일입니다.

- gradle <br>
  gradle이 사용하는 파일입니다.

- src <br>
  main: 실제 소스파일이 있습니다.
  resoruces : java 파일을 제외한 파일
  test: 테스트와 관련된 코드들이 들어감

![image](https://user-images.githubusercontent.com/79408217/155079812-8ad3c4e8-517c-43c4-9c08-f61b970d3387.png)

- build.gradle
  plugins: 지금은 스프링 부트가 가져온 라이브러리라고 간단하게만 이해하고 가면 됩니다.
  sourceCompatibility: Java 버전을 의미합니다.

- repositories
  mavenCentral: dependencies에 있는 라이브러리를 다운받는 공개된 사이트입니다.
  필요하면 특정 사이트를 추가하여 다운을 받을 수도 있습니다.

- dependencies
  위에서 가져온 thymeleaf, web등의 라이브러리에 대한 정보들을 갖고 있습니다.
  testImplementation: 요즘은 보통 테스트 라이브러리가 자동으로 들어갑니다.

![image](https://user-images.githubusercontent.com/79408217/155080722-9a42ec76-803b-4bdc-abba-c61e486db6af.png)

- gitignore
  git에는 소스코드만 올라가야 하므로 빌드된 결과물은 올라가면 안됩니다. 이 과정을 스프링 부트 스타터에서 자동으로 해줍니다.

## 스프링 실행하기

![image](https://user-images.githubusercontent.com/79408217/155081133-5a6264e4-001f-481c-982e-fd599d0bf3b5.png)

main 폴더의 HelloSpringApplication 이라는 클래스가 자동으로 만들어져 있습니다.
여기에는 @SpringBootApplication 이라는 어노테이션(Annotation)이 만들어진 것을 확인할 수 있습니다.
자바는 기본적으로 main 메소드에서 시작합니다.

![image](https://user-images.githubusercontent.com/79408217/155097289-d824f29d-6933-4d62-85ad-016b4c489252.png)

따라서 main 메소드 왼쪽의 초록색 > 표시를 누르고 실행을 누르면 됩니다. 저는 여기서 [**에러**](#에러)가 발생했는데 아래 해결하는 링크를 걸어두었으니 저와 같은 문제가 발생하신 분들은 URL로 이동하여 해결하시기 바랍니다.

![image](https://user-images.githubusercontent.com/79408217/155123688-3860d5e0-fc6c-4d37-b091-ab25c137dc1a.png)

정상적으로 작동을 하면 다음과 같은 화면을 볼 수 있습니다.

먼저 spring 이라는 문구가 나오며 그 아래 버전이 나오는 것을 확인할 수 있습니다.
그리고 맨 아래에서 2번째 줄의 중간을 보면

**Tomcat started on port(s): 8080(http)**

라는 것을 볼 수 있는데 http 프로토콜을 사용하여 8080 포트를 열어놓은 것을 확인할 수 있습니다.

브라우저로 `Localhost:8080` 을 입력하면 다음과 같은 에러페이지 화면을 볼 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155124981-cb0e1504-ea7e-4e59-9ce4-83f7a69d5f6b.png)

이렇게 뜨면 스프링 부트 애플리케이션이 run 하면서 제대로 작동하는 것입니다.<br>
스프링 부트 애플리케이션이 스스로 tomcat 이라는 웹서버를 내장하고 있기 때문에 tomcat이라는 웹서버를 띄우면서 스프링부트를 자체적으로 같이 띄우게 되는 구조입니다.

## 에러

![image](https://user-images.githubusercontent.com/79408217/155122633-574355cd-538f-4ac3-824f-e7712ae8846f.png)
Java HotSpot(TM) 64-Bit Server VM warning: Options -Xverify:none and -noverify were deprecated in JDK 13 and will likely be removed in a future release.

만약 여기서 위와 같은 에러가 뜬다면 다음을 확인해보시기 바랍니다.

<https://www.inflearn.com/questions/116973>
<https://yellowh.tistory.com/105>
<https://int-i.github.io/java/2020-11-21/openjdk-noverify-deprecated/>

![image](https://user-images.githubusercontent.com/79408217/155123047-c9043311-18e9-4083-868f-2e89e1e070e3.png)
![image](https://user-images.githubusercontent.com/79408217/155123117-0f3d4e1e-39fd-4adb-b2e1-07fd593a759a.png)

저는 11버전을 사용해야하는데 15버전을 사용해서 13버전에서 에러가 발생했었습니다. 또한 IntelliJ에서 Gradle과 Project Stucture를 잘못 설정해주어 문제가 발생하였습니다

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
