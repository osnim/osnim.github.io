---
title: Algorithm 3.DFS/BFS
author:
  name: osnim
  link: https://github.com/osnim
date: 2022-03-04 14:37:00 +0900
categories: [Algorithm]
tags: [Algorithm, DFS, BFS]
---

## 탐색 (Search)

많은 양의 데이터 중에서 원하는 데이터를 찾는 과정으로 그래프, 트리등의 자료구조 안에서 **탐색**이라는 과정을 사용합니다.<br>
대표적인 탐색 알고리즘으로는 **DFS**, **BFS**이 있습니다.<br>
이러한 **탐색** 알고리즘을 사용하기 위해서는 **스택(Stack)**과 **큐(Queue)**라는 자료구조를 사용합니다. <br>
**자료구조**: 데이터를 표현하고 관리하고 처리하기 위한 구조<br>
**스택**과 **큐**는 **push(삽입)**, **pop(삭제)** 함수들을 사용합니다.<br>
**push(삽입)**, **pop(삭제)**를 사용하기 위해서는 **오버플로우(Overflow)**, **언더플로우(Underflow)**를 고려해야 합니다.<br>
**오버플로우(Overflow)**: 스택과 큐 등의 자료구조의 용량이 가득 찬 상태에서 push()를 수행할 경우 발생합니다.<br>
**언더플로우(Underflow)**: 스택과 큐 등의 자료구조의 데이터가 없는 상태에서 pop()를 수행할 경우 발생합니다.

## 인접행렬, 인접리스트

**인접행렬(Adjacency Matrix)**: 2차원 배열에 연결된 모든 노드들의 정보를 기록하는 방식으로 메모리 낭비가 심합니다.<br>
**인접리스트(Adjacency List)**: 2중 List로 연결된 노드만 기록하는 방식으로 메모리 관리 측면에서 효율적입니다. 하지만 두 노드가 연결되어있는지 확인하는 것은 인접행렬보다 느립니다.

## DFS (Depth-First-Search, 깊이 우선 탐색)

DFS에서 사용되는 기본 자료구조는 스택(Stack)입니다. DFS는 탐색할 수 있는 최대 깊이 부터 우선 탐색하며 더 이상 탐색할 곳이 없으면 스택에서 값을 하나 빼서 가장 나중에 들어온 노드를 방문하는 방식으로 모든 노드를 순회(Traversal)합니다.

기본적으로 파이썬에서 list는 스택 자료구조로 되어있어 따로 구현하거나 import 하지 않아도 됩니다.

DFS의 동작 순서는 다음과 같습니다.

1. 탐색 시작 노드를 스택에 삽입하고 방문 처리합니다<br>
2. 스택의 최상단 노드에 방문하지 않은 인접노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리합니다<br>
   방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼냅니다.<br>
3. 2번 과정을 더 이상 수행할 수 없을 때 까지 반복합니다

DFS의 탐색 순서를 보면 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156724820-f6ba0625-f764-43a5-acf2-9f6bb0d972f0.png)<br>
DFS는 일반적으로 인전합 노드 중에서 방문하지 않은 노드가 여러 개 있으면 숫자가 낮은 노드 부터 탐색합니다.

지금부터 DFS가 자세하게 어떤 식으로 탐색하는지 알아보도록 하겠습니다.

![image](https://user-images.githubusercontent.com/79408217/156725938-02332f31-65c1-429d-83a7-1fa76cc48e9b.png)

먼저 시작 노드를 '1'을 스택에 넣고 방문처리 합니다. 스택의 최상단 노드인 '1'에 방문하지 않은 인접노드 '2', '6', '8' 중에서 가장 낮은 번호를 가진 노드를 우선 탐색하고 stack에 '2'를 삽입합니다.<br>
스택의 최상단 노드인 '2'번 노드와 인전합 노드중 가장 낮은 번호의 노드인 '3'을 stack에 넣고 '3'번 노드를 방문 처리합니다.

![image](https://user-images.githubusercontent.com/79408217/156726383-40165c85-5fef-40c8-ba8f-0b2a5cf5160e.png)

'3'번 노드의 인접 노드인 '4'번 노드를 스택에 넣고 방문처리합니다. 이런식으로 더 이상 깊게 탐색할 수 없을 때 까지 반복합니다.<br>
'5'번 부터 방문할 수 있는 인접 노드가 없기 때문에 '5'번 노드를 스택에서 꺼냅니다.<br>
이런 방식으로 '2'번 노드만 남기고 모두 스택에서 꺼냅니다.

![image](https://user-images.githubusercontent.com/79408217/156733886-ba99b6bd-ed39-47e5-9a92-905d25f110ae.png)

'6'번 노드에서 인접한 노드 '7', '8'번 노드 중에서 숫자가 낮은 순서대로 스택에 넣고 탐색을 합니다. (i) 번째 그림은 최종 DFS의 탐색 순서 및 결과입니다.

DFS를 python 코드로 나타내면 다음과 같습니다.

```python
def dfs(graph, v, visited):
   visited[v] = True
   print(v, end='')

   for i in graph[v]:
      if not visited[i]:
         dfs(graph, i, visited)

graph = [
   [],
[2, 6 ,8],
[1, 3, 5],
[2, 5],
[3, 5],
[2, 3, 4],
[1, 2, 7, 8],
[6],
[1, 6] ]

visited = [False]*9

dfs(graph, 1, visited)
```

## BFS (Breadth-First-Search, 너비 우선 탐색)

BFS는 가까운 노드부터 탐색하는 알고리즘으로 큐(Queue)라는 자료구조를 이용합니다.<br>
인접한 노드를 반복적으로 큐에 넣어 먼저 들어온 노드를 먼저 나가도록 구현하여 가까운 노드부터 탐색하면서 모든 노드를 순회(Traversal)합니다.

BFS의 동작 순서는 다음과 같습니다.

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 합니다.<br>
2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문처리를 합니다.<br>
3. 2번 과정을 수행할 수 없을 때까지 반복합니다.<br>

기본적으로 파이썬에서는 큐를 구현하지 않고 Queue 라이브러리를 사용하여 큐 자료구조를 이용합니다.

BFS의 탐색 순서를 보면 다음과 같습니다.

![image](https://user-images.githubusercontent.com/79408217/156742545-a52930a4-4623-4213-981d-3989a22cf0b4.png)

BFS가 어떻게 작동하는지 구체적으로 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/79408217/156742785-e86a489e-fa04-4548-9058-2895454eae96.png)

먼저 시작노드인 '1'번 노드를 큐에 삽입하고 방문 처리합니다.<br>
큐에서 노드 '1'을 꺼내고 방문하지 않은 인접노드 '2', '3', '4'를 큐에 삽입합니다.<br>
인접노드 중에서 가장 먼저 들어온 '2'번 노드를 방문 처리하고 큐에서 pop합니다.<br> 그 후 인접노드를 삽입합니다. 하지만 2번 노드는 모두 큐에 있거나 삽입 되어있어 추가되는 노드는 없습니다.<br>
'3'번 인접노드인 '5'번 '4'번 큐에 삽입하고 방문 처리합니다. 그 후 '4'번 인접노드인 '6', '7'번 노드를 큐에 삽입합니다.<br>

![image](https://user-images.githubusercontent.com/79408217/156753958-f0c92c48-15a5-4f2b-beec-ca96fb6c8b78.png)

위와 같은 방식으로 큐에 먼저 들어온 노드들을 탐색하여 BFS를 마무리합니다.<br>
마지막 그림은 BFS의 탐색 순서를 나타낸 그림입니다.

BFS를 python 코드로 나타내면 다음과 같습니다.

```python
from collections import deque

def BFS(graph, start, visited):
   queue = deque([start])
   visited[start] = True
   while queue:
      v = queue.popleft()
      print(v, end='')

      for i in graph[v]:
         if not visited[i]:
            queue.append(i)
            visited[i] = True

graph = [
[],
[2, 6 ,8],
[1, 3, 5],
[2, 5],
[3, 5],
[2, 3, 4],
[1, 2, 7, 8],
[6],
[1, 6] ]

visited = [False]*(9)

BFS(graph, 1, visited)
```

#### DFS, BFS 탐색 비교

![image](https://user-images.githubusercontent.com/79408217/156755906-76b3ceb3-7df4-4838-af9b-cf5c634c2b78.png)

동일한 트리에서 DFS, BFS 탐색 순서를 비교한 그림입니다. 이해에 조금이라도 도움을 드리고자 첨부하였습니다.

## 참조

본 글은 제가 전공수업으로 들었던 '빅데이터처리및응용'과 나동빈 저 '이것이 코딩테스트다 with 파이썬'를 바탕으로 공부하여 정리한 내용입니다.
