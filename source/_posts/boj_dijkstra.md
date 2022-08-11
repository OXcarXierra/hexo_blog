---
title: Dijkstra 알고리즘 (최단 경로 문제)
tags: [dijkstra algorithm, graph]
categories: [Algorithm]
thumbnailImage: https://i.imgur.com/ydX4P1X.png
thumbnailImagePosition: right
permalink: ''
date: 2022-07-26 22:27:45
---

**BOJ 1753번 최단경로**

<!-- excerpt -->
<!-- toc -->

## Dijkstra Algorithm이란?

그래프의 모든 단선이 음이 아닌 가중치를 가졌을 때 노드와 노드 사이의 최단경로를 구하는 알고리즘이다.

→ 다른 알고리즘으로는 플로이드-워셜 알고리즘이 있다. 이건 모든 노드쌍 사이의 최단경로 알고리즘.
→ 단선에 음의 가중치가 있을 땐 벨만-포드 알고리즘.

노드의 개수 V, 단선의 개수 E인 그래프에 가중치가 주어졌고, 각 노드까지의 최단경로를 저장할 distance list를 선언했다고 가정하자.

1. 출발 노드를 설정
2. 출발 노드와 연결된 노드들의 distance 설정 (최단거리라는 보장은 없다)
3. 방문하지 않은 노드들 중 출발 노드와 최단거리가 가장 작은 노드를 선택한다.
4. 이 노드를 거치는 경로를 계산했을 때 더 짧은 거리가 생성된다면 distance를 고친다.
5. 3,4과정을 모든 노드에 대해 반복한다.

핵심은, 3) 방문하지 않은 노드 중 최단거리의 노드를 선택하는 과정을 반복하는 데에 있다. 문제 안의 소문제를 찾아서 해결하는 DP의 응용이기도 하다.

이 과정에서 단순 탐색을 이용하는 방법과, 우선순위 큐를 이용하는 방법이 있다.

## 단순탐색을 이용한 Dijkstra Algorithm

이 때는 각 노드의 방문여부를 flag로 만들어준다.

```python
INF = int(1e9)

V, E = map(int, input().split())
K = int(input())
graph= [[] for _ in range(V+1)]
for _ in range(E):
  u, v, w= map(int, input().split())
  graph[u].append((v,w))

distance = [INF]*(V+1)
visited = [False]*(V+1)

def get_smallest_node():
  min_value = INF
  index = 0
  for i in range(1, V+1):
    if not visited[i] and distance[i] < min_value:
      index = i
      min_value = distance[i]
  return index

def dijkstra():
  visited[K] = True
  distance[K] = 0
  for (i,v) in graph[K]:
    distance[i] = v
  for _ in range(V-1):
    i = get_smallest_node()
    visited[i] = True
    for (j,v) in graph[i]:
      if distance[j] > distance[i] + v:
        distance[j] = distance[i]+ v

dijkstra()

for i in range(1, V+1):
  if distance[i] == INF:
    print('INF')
  else:
    print(distance[i])
```

시간복잡도는 O(V^2)이다. 왜냐하면 V개의 모든 노드에 대해 3)과정을 거치며 V번의 비교를 하게 되기 때문.

## 우선순위 큐(최소 힙)을 이용하는 Dijkstra Algorithm

이 과정에서 우선순위 큐(최소 힙)을 이용한다면, 복잡도를 줄일 수 있다.
아래 코드는 BOJ 1753 최단경로의 답이기도 하다.

```python
import heapq

INF = int(1e9)

V, E = map(int, input().split())
K = int(input())
graph= [[] for _ in range(V+1)]
for _ in range(E):
  u, v, w= map(int, input().split())
  graph[u].append((v,w))

distance = [INF]*(V+1)

def dijkstra():
  q = []
  distance[K] = 0
  heapq.heappush(q, (0, K))
  while q:
    dist, node = heapq.heappop(q)
    if distance[node] < dist:
      continue
    for (j,v) in graph[node]:
      cost = distance[node]+ v
      if distance[j] > cost:
        distance[j] = cost
        heapq.heappush(q, (cost, j))

dijkstra()

for i in range(1, V+1):
  if distance[i] == INF:
    print('INF')
  else:
    print(distance[i])
```

이 과정에서는 최단거리 노드를 찾는 데에 O(VlogV)가 필요하고(V개의 노드에 대해 힙에서 추출하는 시간 logV, 각 노드가 인접한 노드를 갱신할 때 (visited를 체크하지 않으므로) 모든 간선을 확인하는 것과 같으며 갱신될 때 logV가 필요하므로 O(ElogV)가 필요하다. 결과적으론 O((E+V)logV)가 필요한 셈.

이 부분에서 헷갈린게 있었는데, 최악의 경우 E는 V^2 스케일일 때 복잡도를 비교하면 전자가 더 빠르다. 즉 그래프가 sparse할 때만 효율적이다.

> 참고문헌
> https://techblog-history-younghunjo1.tistory.com/248?category=1014800
> 위 블로그가 정말 설명이 깔끔하게 잘 되어있다. 다른 알고리즘도 참고해 보아야겠다
> https://namu.wiki/w/다익스트라 알고리즘
