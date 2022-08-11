---
title: 위상 정렬 알고리즘 Topological Sort
tags: [graph, sort]
categories: [Algorithm]
permalink: ''
thumbnailImage: https://i.imgur.com/jOe1hdP.png
thumbnailImagePosition: right
date: 2022-08-09 15:03:19
---

**BOJ 2252 줄세우기**, **BOJ 1766 문제집**

<!-- excerpt -->
<!-- toc -->

---

## 위상 정렬 _Topological Sort_

그래프 중 비순환 방향 그래프(DAG)에 대해, 간선 (u,v)가 있으면 u가 v보다 먼저 오도록 **모든 정점을 선형으로 정렬하는 알고리즘**이다. 대부분의 문제에서, 원소들 사이에 **우선순위가 있는 데이터의 순서를 정하는 데에 사용**될 수 있다.
한편 그래프에 순환이 존재한다면, 당연하게도 그 순환을 선형으로 표현할 수 없기 때문에 위상 정렬 알고리즘을 적용할 수 없다. 또한 하나의 그래프에서 다수의 위상 순서 *Topological Order*가 나올 수 있다.

## BFS를 이용한 in-degree 방법

1. 정점 N개에 대해 방문했는지 여부를 표시할 배열 **visited**, 출력배열 **t**, **deque**를 초기화한다.
1. 정점 N개에 대해 해당 정점으로 들어오는 간선의 개수를 **in_degree** 배열에 저장한다.
1. in_degree가 0인 원소들부터 deque에 넣는다. 없다면 반드시 사이클이 존재하므로 위상 정렬을 실핼할 수 없다.
1. deque의 왼쪽에서 원소 하나를 pop해서, 출력 배열 t에 추가한다. 또, 해당 정점과 연결된 정점들의 in_degree를 1 감소시킨다. 만약 그 값이 0이 되었다면 deque의 오른쪽에 추가한다. 또한 그 노드의 visited값을 True로 저장한다.
1. 3~4의 과정을 deque가 빌 때까지 반복한다.

만약 출력배열에 모든 정점이 출력되지 않았다면, DAG가 아니었던 것으로 판단할 수 있다. 이 사실을 이용해 [BOJ 2623 음악 프로그램]('https://www.acmicpc.net/problem/2623')을 해결할 수 있다.

### BOJ 2252 줄세우기

```python BOJ 2252 줄세우기
from collections import deque

n, m = map(int, input().split())
graph = [[] for _ in range(n+1)]
deg = [0]*(n+1)
for _ in range(m):
  a,b = map(int, input().split())
  graph[a].append(b)
  deg[b] +=1

visited = [False]*(n+1)
t = []
q = deque([])

for i in range(1,n+1):
  if deg[i] == 0:
    deque.append(q,i)
    visited[i] = True

while q:
  node = deque.popleft(q)
  t.append(node)
  for next in graph[node]:
    if not visited[next]:
      deg[next] -= 1
      if deg[next] == 0:
        visited[next] = True
        deque.append(q, next)

print(*t)
```

### 우선순위 힙을 접목한 in-degree 방법 (BOJ 1766 문제집)

[BOJ 1766 문제집]('https://www.acmicpc.net/problem/1766') 문제는 한 정점 뒤로 올 수 있는 모든 정점 중 **최솟값**이 그 다음에 와야만 하는 조건이 추가되어 있다. 이 경우엔 간단하게 deque를 heapq클래스로 바꿔주면 쉽게 해결할 수 있다. in-degree가 0이 되면 대기열에서 크기 순서대로 정렬되어 가장 크기가 작은 원소부터 출력 배열에 저장되게 된다.

```py BOJ 1766 문제집
import heapq

n, m = map(int, input().split())
graph = [[] for _ in range(n+1)]
indeg = [0]*(n+1)
for _ in range(m):
  a, b= map(int, input().split())
  graph[a].append(b)
  indeg[b] += 1

visited = [False]*(n+1)
t , q =[], []

for i in range(1, n+1):
  if indeg[i] == 0:
    heapq.heappush(q,i)
    visited[i] = True

while q:
  node = heapq.heappop(q)
  t.append(node)
  for next in graph[node]:
    indeg[next] -= 1
    if indeg[next] == 0:
      visited[next] = True
      heapq.heappush(q, next)

print(*t)
```

<!-- ## DFS를 이용한 방법 -->

> 참고문헌
> https://yoongrammer.tistory.com/86
