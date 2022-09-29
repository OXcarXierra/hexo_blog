---
title: Kruskal 알고리즘 (최소 스패닝 트리 문제)
tags: [graph, mst]
categories: [Algorithm]
permalink: ''
thumbnailImage: https://i.imgur.com/jOe1hdP.png
thumbnailImagePosition: right
date: 2022-08-07 16:37:56
---

**BOJ 1197번 최소 스패닝 트리**

<!-- excerpt -->
<!-- toc -->

---

## 최소 스패닝 트리 (Minimum Spanning Tree, MST)

**스패닝 트리 Spanning tree**는 무향 그래프의 정점 전부와 간선의 부분집합으로 구성되어 있으면서, 모든 정점이 연결된 부분 그래프로 정의한다. 연결되기만 하면 되는 점에서 당연히 스패닝 트리는 하나가 아니다. 이 중 간선에 가중치가 부여된 경우 **가중치의 합이 가장 작은 스패닝 트리**를 찾는 문제가 **최소 스패닝 트리 Minimum Spanning Tree** 문제이다.
한마디로, 그래프의 연결성을 그대로 유지하는 가장 '저렴한' 그래프를 찾는 문제라고 할 수 있다.

최소 스패닝 트리를 찾는 두 가지 유명한 알고리즘이 있는데, **Kruskal 알고리즘**과 **Prim 알고리즘**이 그것이다. 둘 다 비용이 적은 간선을 우선적으로 선택하는 **탐욕법**의 본질을 가지고 있다.

## Kruskal 알고리즘

Kruskal 알고리즘은 가중치가 낮은 간선부터 하나씩 추가해가면서 스패닝 트리를 만들어간다. 하나의 간선을 추가했을 때 최소 스패닝 트리의 조건을 만족하는지 검사해야 하는데, 이 때 {% post_link boj-disjoint-set Union-Find 자료구조 %}가 유용하게 쓰인다. 해당 간선을 추가했을 때 그래프에 사이클이 생긴다면, 불필요한 간선이므로 추가할 필요가 없기 때문이다.
<br>

1. 그래프의 모든 정점을 Union-Find 자료구조로 구현한다. 처음에는 원소 각각이 상호 배타적 부분집합이 된다.
1. 가중치가 가장 낮은 간선부터 검사한다. 두 정점이 A,B라고 가정하면 **Find(A)와 Find(B)가 다른 경우에만 Union(A, B)를 실행**한다. 그리고 해당 간선의 가중치를 더한다.
1. 2번 과정을 모든 간선에 대해 반복한다.

## Kruskal 알고리즘의 시간 복잡도

우선 Union, Find연산의 시간 복잡도는 **상수 시간으로 수렴**하므로 전체 알고리즘의 복잡도에 영향을 미치지 못한다. 따라서 정렬된 (`O(log E)`) 모든 간선에 대해(`O(E)`) 검사를 진행해야 하므로, 전체 시간 복잡도는 `O(ElogE)`가 된다.

## BOJ 1197번 최소 스패닝 트리

```py BOJ 1197번 최소 스패닝 트리
import sys
input = sys.stdin.readline
V, E = map(int, input().split())
graph = []
for _ in range(E):
  s,e,w = map(int, input().split())
  graph.append([w,s,e])
parent = [i for i in range(V+1)]
rank = [0 for _ in range(V+1)]

def find(a):
  if a == parent[a]:
    return a
  else:
    parent[a] = find(parent[a])
    return parent[a]

def union(a,b):
  a, b = find(a), find(b)
  if a==b:
    return -1
  if rank[a] > rank[b]:
    a, b = b, a
  parent[a] = b
  if rank[a] == rank[b]:
    rank[b] += 1

def kruskal():
  tot_weight = 0
  graph.sort()
  for w,s,e in graph:
    if find(s) != find(e):
      union(s,e)
      tot_weight += w
  return tot_weight

print(kruskal())
```

> 참고자료
> 프로그래밍 대회에서 배우는 알고리즘 문제해결전략 (종만북), 31장
