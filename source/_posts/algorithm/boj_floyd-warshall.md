---
title: Floyd-Warshall 알고리즘 (최단 경로 문제)
tags: [floyd-warshall, graph]
categories: [Algorithm]
thumbnailImage: 'https://i.imgur.com/ydX4P1X.png'
permalink: ''
date: 2022-07-28 10:43:12
---

**BOJ 11404번 플로이드**

<!-- excerpt -->
<!-- toc -->

## Floyd-Warshall 알고리즘이란

Dijkstra 알고리즘이 그래프의 정해진 한 노드에서 다른 노드까지의 최단거리를 구하는 알고리즘이었다면, Floyd-Warshall 알고리즘은 출발 노드가 따로 정해져있지 않다. 따라서 V개의 노드가 있다면 V^2개의 최단경로를 결과로 얻게 된다.

이 알고리즘의 핵심은 각 노드를 돌며 **이 노드를 지나는 경로가 더 짧을 경우 갱신**헤주는 방식이다.

1. 최단경로를 저장할 V by V 인접행렬을 만들고, INF값으로 초기화시킨다.
1. 입력값을 행렬에 저장한다. 대각선 원소들에는 0을 저장한다.
1. 각 노드를 순환하며, 다른 두 노드 사이의 최단거리가 이 노드를 거쳐갈 경우에 거리가 짧아진다면 갱신한다. (min함수를 이용한다)

## Floyd-Warshall 알고리즘의 시간복잡도

단순하게 생각하면, V개의 노드를 세 번 순환을 돌았으니 O(V^3)이다.

## BOJ 11404 플로이드

```python
INF = int(1e9)
n = int(input())
m = int(input())
dist = [[INF]*(n+1) for _ in range(n+1)]

for _ in range(m):
  a, b, c= map(int, input().split())
  dist[a][b] = min(dist[a][b],c)
for i in range(n+1):
  dist[i][i] = 0

for i in range(1,n+1):
  for j in range(1,n+1):
    for k in range(1,n+1):
      dist[j][k] = min(dist[j][k],dist[j][i] + dist[i][k])

for z in range(1,n+1):
    for y in range(1, n+1):
        if dist[z][y] == INF:
            dist[z][y] = 0
    print(*dist[z][1:],sep=" ")
```

BOJ 11404에선 입력값에 같은 간선이 들어올 수 있으므로 입력받을 때에도 min함수를 이용해 비교해주어야 한다.

> 참고자료
> https://namu.wiki/w/플로이드-워셜%20알고리즘
