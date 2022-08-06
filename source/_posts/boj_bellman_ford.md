---
title: Bellman-Ford 알고리즘
tags: [bellman-ford, graph]
categories: [Algorithm]
thumbnailImage: https://i.imgur.com/ydX4P1X.png
thumbnailImagePosition: right
permalink: ''
date: 2022-08-02 16:52:10
---

**BOJ 1865 웜홀, BOJ 11657 타임머신**

<!-- excerpt -->

<!-- toc -->

## Bellman-Ford 알고리즘

가중치 그래프의 한 노드에서 다른 노드까지의 최단경로를 찾는 탐색 알고리즘이다. Dijkstra 알고리즘과 차이점은, 가중치가 음수인 간선이 있을 때도 적용 가능한 알고리즘이다.

**음의 사이클**이 발생할 경우, 최단경로를 찾을 수 없다. 최단거리를 무한으로 감소시킬 수 있기 때문이다.
Bellman-Ford 알고리즘으로 이런 음의 사이클이 존재하는지도 찾아낼 수 있다. (BOJ 1865 웜홀이 이같은 문제)

기본적인 알고리즘은 이렇다.

1. 출발 노드를 설정한다.
1. 최단거리 배열을 초기회한다. (출발노드의 최단거리 = 0, 나머지는 매우 큰 수 INF)
1. 모든 간선들에 대해 해당 간선을 거쳤을 때 최단거리가 짧아지는지 검사하고, 짧아진다면 갱신한다.
1. 이 과정을 (노드 개수)번 반복한다.
1. 최단경로의 길이는 최대 (노드 개수)-1인데, 그보다 많이 이동하고 최단거리가 갱신된다면 음의 사이클이 존재한다고 판단할 수 있다. 즉, 마지막 반복에 최단거리 배열이 갱신된다면, -1을 return한다.

## BOJ 11657 타임머신

Bellman-Ford 알고리즘을 적용하면 쉽게 해결할 수 있다.

```python
INF = int(1e9)
n, m = map(int, input().split())
graph = []
for _ in range(m):
    graph.append(list(map(int, input().split())))

def bf():
    dist = [INF]*(n+1)
    dist[1] = 0
    for i in range(n):
        for [s,e,w] in graph:
            if dist[s] != INF and dist[s]+w < dist[e]:
                dist[e] = dist[s]+w
                if i == n-1:
                    print(-1)
                    return
    for j in range(2,n+1):
        print(-1 if dist[j] == INF else dist[j] )

bf()
```

## BOJ 1865 웜홀

이 문제에서는 **음수 사이클**의 존재 여부만 반환하면 된다. 쉬워 보이지만, 출발 노드가 정해지지 않았다는 점을 잘 고려해야 한다.
만약 출발 노드를 임의로 1번으로 지정한 경우, 1번 노드와 간선으로 연결되어있지 않은 노드들의 음수 사이클 존재를 알아낼 수 없다.
그래서 `if dist[s] != INF and dist[s]+w < dist[e]:` 조건문을 수정해주었는데, 이렇게 하면 음수 사이클을 돌았을 때 값이 INF보다 작아지게 되어 존재 여부를 판단할 수 있다.

```python
TC = int(input())
INF = int(1e9)

def bellman_ford(n, graph, start):
  dist = [INF]*(n+1)
  dist[start] = 0
  for i in range(n+1):
    for [s,e,t] in graph:
      if dist[e] > dist[s] + t:
        dist[e] = dist[s] + t
        if i == n:
          return 'YES'
  return 'NO'

for _ in range(TC):
  n, m, w = map(int, input().split())
  graph = []
  for _ in range(m):
    s, e, t = map(int, input().split())
    graph.append([s,e,t])
    graph.append([e,s,t])
  for _ in range(w):
    s, e, t = map(int, input().split())
    graph.append([s,e,-t])
  print(bellman_ford(n,graph, 1))
```

~~'Yes' 'No'로 적고 왜 틀렸습니다가 뜨는지 30분 삽질했다..~~
