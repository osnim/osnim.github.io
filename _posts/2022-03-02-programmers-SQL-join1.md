---
title: 프로그래머스 - SQL(JOIN-1) 없어진 기록 찾기
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-02 18:33:00 +0900
categories: [SQL]
tags: [Programmers, SQL, MySQL]
---

## SQL JOIN

SQL에서 JOIN이란 두 개 이상의 테이블(릴레이션)을 연결하여 데이터를 검색하는 방법으로 두 테이블에서 관련된 튜플을 결합하여 하나의 튜플로 만들어 출력합니다.

JOIN은 DML중 SELECT에서 조건을 추가할 때 사용하는 방법으로 보통 기본키(PK)와 외래키를 사용하여 JOIN을 합니다.

JOIN의 종류를 밴 다이어그램으로 표현하면 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156342102-d2b527d9-292e-4f77-99df-14823e637f80.png)

위 그림을 `SQL`로 표현하면 다음과 같습니다.

```SQL
  SELECT <필드 명...>
  FROM [기준 테이블 명] LEFT JOIN [JOIN 되는 테이블]
  ON [기준 테이블.외래키] = [JOIN 테이블.기본키]
```

- 꼭 외래키와 기본키일 필요는 없지만 대부분 외래키, 기본키를 이용하여 코드를 작성한다고 생각하여 위와 같이 작성했습니다.

- `RIGHT JOIN`은 `LEFT JOIN`에서 기준 테이블만 반대로 하면 되는거라서 자세하게 적지 않았습니다.

- on 은 where와 같은 역할을 하지만 자세한 차이점은 on은 레코드가 생성될 때 레코드를 필터링하는 반면에, where는 조인작업이 완료될 때까지 기다리고 난 뒤에 필터링을 합니다.

## 문제

출처: <https://programmers.co.kr/learn/courses/30/lessons/59042>

![image](https://user-images.githubusercontent.com/79408217/156336742-8c89406c-eea6-4a70-999c-ea9d9a9e6b41.png)

![image](https://user-images.githubusercontent.com/79408217/156336885-bc9a84b4-54d2-4012-b8ee-f9be077d7177.png)

![image](https://user-images.githubusercontent.com/79408217/156336930-be2bb8fa-c436-4472-8abc-86c5a9c49c58.png)

## 풀이 및 오답 정리

![image](https://user-images.githubusercontent.com/79408217/156363852-404b2d23-cbdd-4028-b5f0-730dc455bbd4.png)

이 문제는 `ANIMAL_OUTS` 테이블에는 동물들의 ID가 있는데 `ANIMAL_INS`에 없는 동물들의 ID를 찾는 방식으로 ANIMAL_OUTS 차집합을 찾아 풀어야합니다.

JOIN은 대부분 있을 때 A테이블에 있고 B테이블에 있을 때 `=`를 사용한다는 것을 파악했습니다.

이번 문제는 없는 경우를 찾는 것이라서 다음과 같이 작성하였습니다. `!=` 문법이 없으면 에러가 발생할 것이라고 생각해서 다음과 같이 코드를 작성하고 결과를 보았습니다.

```SQL
  SELECT ANIMAL_OUTS.ANIMAL_ID, ANIMAL_OUTS.NAME
  FROM ANIMAL_OUTS LEFT JOIN ANIMAL_INS
  ON ANIMAL_OUTS.ANIMAL_ID != ANIMAL_INS.ANIMAL_ID
```

결과는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156351407-3ae93772-2657-482e-9436-79da23e3f6c3.png)

중복된 값이 나오지만 제대로 출력이 나온다고 생각해서 `!=`에서는 문제가 없다고 생각해서 DISTINCT 만 추가하면 되는 줄 알았습니다.

하지만 DISTINCT를 추가해도 문제는 계속 틀렸습니다.

제가 착각하고 있었던 부분은 `JOIN`에서 기준 테이블의 키가 비교되는 테이블에 없으면 그 튜플은 출력이 안되는 줄 알았습니다.

하지만 아래 그림에서 처럼 NULL 값으로 대체되어 출력되는 것을 새롭게 알게 되었습니다.

`!=` 에서 문제가 있을 것이라고 생각하고 JOIN에 대한 여러 예제를 찾아보고 [w3schools]<https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all> 에서 기본 제공하는 테이블을 이용해서 직접 JOIN을 이용해서 튜플들을 출력해 보았습니다.

제가 사용한 두 개의 테이블은 다음과 같습니다.

- Categories 테이블

  ![image](https://user-images.githubusercontent.com/79408217/156362584-ff895424-11be-4c71-9a88-abeccccec121.png)

- Shippers 테이블

  ![image](https://user-images.githubusercontent.com/79408217/156362688-25c16c1c-d6f3-4268-934c-9f0d70a8b011.png)

  결과는 다음과 같았습니다.

![image](https://user-images.githubusercontent.com/79408217/156352993-6ddcc8ad-3b5d-41be-bc69-a3e74d68ea05.png)

바로 값이 없는 경우 **NULL**을 출력하여 기준 테이블에 맞춰 튜플이 출력되는 것을 확인할 수 있었습니다. 그래서 이 경우는 **NULL** 존재하는 경우만 출력해야 합니다. 따라서 `JOIN`에 `WHERE`절을 추가하고 값이 없는 경우, `!=`이 아니라 `NULL` 이라는 키워드를 사용해서 튜플이 없는 경우는 출력시키지 않아야 합니다.

## 정답 코드

```SQL
  SELECT ANIMAL_OUTS.ANIMAL_ID, ANIMAL_OUTS.NAME
  FROM ANIMAL_OUTS
  LEFT JOIN ANIMAL_INS
  ON ANIMAL_OUTS.ANIMAL_ID = ANIMAL_INS.ANIMAL_ID
  WHERE ANIMAL_INS.ANIMAL_ID IS NULL
```

정답을 알고나면 쉬워 보이지만 처음 JOIN을 이용한 저로서는 왜 `WHERE절`이 필요한지 궁금증이 생겨 깊게 공부하고 그 내용을 정리하면서 글을 작성하게 되었습니다.

## 참고

<http://aispiration.com/xwmooc-sc/novice/sql/07-join.html>

## SQL 실습 사이트

1. [w3schools]<https://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all>

   테이블이 여러 개 있어 JOIN등 튜플을 조합연습을 하기 좋은 사이트

2. [sqltest]<http://sqlfiddle.com/>

   테이블 추가가 가능한 사이트
