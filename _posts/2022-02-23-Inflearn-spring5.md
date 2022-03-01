---
title: 스프링 입문 3 - 2.프로젝트 환경설정 - 빌드하고 실행하기
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-02-23 14:00:00 +0900
categories: [Back-end]
tags: [Inflearn, Spring]
---

## 시스템 환경

제 노트북 환경은 다음과 같습니다.

java 11, windows 10

## gradlew로 빌드하기

저는 프로젝트 `Hellospring` 폴더 주소창에서 cmd 를 입력하고 gradlew 로 빌드를 했더니 다음과 같은 에러가 발생했습니다.

```console
  ERROR: JAVA_HOME is set to an invalid directory: ~~~~~

  Please set the JAVA_HOME variable in your environment to match the
  location of your Java installation.
```

이는 Java 환경 변수 경로를 잘못 설정한 에러때문에 발생하는것으로 파악됩니다.
저도 초기에 설명을 따라 JAVA_HOME의 주소를

C:\Program Files\Java\jdk-11.0.14\bin

까지 설정하였지만 아래 사진처럼 bin을 삭제하니 정상적으로 gradlew로 빌드할 수 있었습니다.

![image](https://user-images.githubusercontent.com/79408217/155293151-a092aaf2-8c7b-4932-bdd2-b22cddf046b8.png)

![image](https://user-images.githubusercontent.com/79408217/155293058-534f1eb1-a002-42a4-b8f4-1a5a731aa6c2.png)

그 후 콘솔창에 다음 명령어를 입력하면 build 폴더가 다음과 같이 생성됩니다.

```console
  gradlew build
```

![image](https://user-images.githubusercontent.com/79408217/155293884-2ddd0e07-565b-40a1-b1a2-0cc74e292192.png)

빌드가 정상적으로 작동되면 cd build를 cmd에서 입력하여 build 후 생성된 build 폴더의 libs 폴더로 이동을 합니다.

![image](https://user-images.githubusercontent.com/79408217/155294208-b5ee1882-1d52-4321-96d7-b642ef1b4c26.png)

libs 폴더로 이동을 하면 약 19M 정도의 jar 파일이 하나 생성되는 것을 확인할 수 있습니다.

그 후 다음과 같은 명령어를 입력하여 jar 파일을 실행합니다.

```console
  java -jar {프로젝트이름}-0.0.1-SNAPSHOT.jar
```

![image](https://user-images.githubusercontent.com/79408217/155294809-336b5621-2836-4bbd-9ca2-7366c1a7af57.png)

![image](https://user-images.githubusercontent.com/79408217/155295000-5e351fee-e186-4d95-8116-c3768d56d654.png)

IntelliJ에서 main 메소드를 run을 실행한 결과와 똑같은 화면을 볼 수 있습니다.

![image](https://user-images.githubusercontent.com/79408217/155295876-8ea97584-9c71-4858-850d-17c931f89a41.png)

![image](https://user-images.githubusercontent.com/79408217/155295891-a9346d81-2d3d-4fff-ac34-80a410542eaa.png)

서버를 배포할 때는 **{프로젝트이름}-0.0.1-SNAPSHOT.jar** 파일만 복사하여 서버에 넣고 `java -jar` 으로 실행하면 됩니다.
이렇게 하면 서버에서 스프링이 알아서 동작합니다.

## build 폴더 삭제

```console
  gradlew clean
```

이 글은 *김영한님*의 _스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술_ 강의를 직접 정리한 내용입니다.

출처: <https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum>
