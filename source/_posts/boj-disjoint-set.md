---
title: Union-Find 자료구조 (상호 배타적 집합 Disjoint Set)
tags: [data structure]
categories: [Algorithm]
permalink: ''
thumbnailImage: https://i.imgur.com/jOe1hdP.png
thumbnailImagePosition: right
date: 2022-08-07 14:40:33
---

**BOJ 20040 사이클 게임**

<!-- excerpt -->
<!-- toc -->

---

## 상호 배타적 집합 Disjoint Set과 Union-Find 자료구조

집합의 원소들을 여러 개의 부분집합으로 나누되, 그 부분집합들의 교집합이 없도록 분류하는 상황을 가정해 보자. 이러한 부분집합들을 **상호 배타적 집합**(혹은 분리집합, Disjoint Set)이라고 하는데, 이 상황을 구현하기 위해 고안된 자료구조가 **Union-Find 자료구조**이다.

Union-Find 자료구조는 세 가지 연산이 가능하다.

- 초기화 : n개의 원소가 각각의 집합에 들어있을 수 있도록 초기화
- **Find 연산** : 주어진 원소가 속한 집합을 찾아(Find) 반환
- **Union 연산** : 주어진 두 원소가 속한 집합을 하나로 합침(Union)

### 배열로 구현한 Union-Find

쉽게 생각해보면, Union-Find는 `arr[i] = i가 속한 집합의 번호` 인 1차원 배열로 생각할 수 있다. 이 경우 Find연산은 배열에 직접 접근해서 찾으면 되니`O(1)`의 복잡도를 가진다. 그러나, Union연산은 전체 배열을 훑으며 a번 집합에 속한 모든 원소를 b로 바꿔주어야 하기 때문에, `O(n)`의 복잡도를 가진다.

### 트리를 이용해 구현한 Union-Find

Find에 걸리는 시간을 조금 늘리더라도, Union을 빠르게 할 수 있도록 고안된 것이 트리를 활용한 방법이다. **하나의 집합에 속한 원소들을 트리로 연결**하면, 각각의 집합은 서로 다른 루트를 가진 독립적인 트리로 생각할 수 있다. Find연산의 경우, 해당 원소의 **부모 노드를 따라가서 루트에 도달**하면 어떤 집합에 속해있는지 알 수 있다. 또한 Union연산은 두 원소가 속한 집합을 Find로 찾아낸 후 **한 쪽 트리를 다른 쪽 트리의 아래로 합치면** 된다.

```python 트리를 이용해 구현한 Union-Find
def find(a):
  if a == parent[a]: # 원소가 트리의 루트인 경우
    return a
  else:
    return find(parent[a]) # 트리를 재귀적으로 따라 올라가 루트 원소를 반환

def union(a,b):
  a = find(a)
  b = find(b)
  if a==b:
    return -1 # 합치려는 두 집합이 같은 경우
  parent[a] = b
```

이 경우 Find, Union 연산 모두 트리의 높이에 비례한 복잡도를 가진다. `O(n)`의 복잡도를 가졌던 1차원 배열보다 훨씬 빨라진 것을 알 수 있다.

## Union-Find의 최적화

1부터 n까지 n개의 원소로 초기화된 상호 배타적 집합을 Union-Find 구조로 구현된 상황을 생각해보자. 1,2를 합치고, 2,3을 합치고... i와 i+1을 합치는 반복적인 연산을 한다면, 트리의 깊이는 최대 n까지 늘어난다. 그려면 Find연산의 복잡도도 `O(n)`, Union연산의 복잡도도 `O(n)`으로 오히려 효율이 나빠진다. 이렇게 트리가 한쪽으로 기울어지는 문제를 해결하고 구조를 최적화하는 방법은 Union-by-rank, 경로압축으로 두 가지가 있다.

### Union-by-rank

트리를 합치는 과정에서 높이가 낮은 트리를 높은 트리 밑으로 집어넣는다면, 두 트리의 높이가 같지 않는 이상 트리의 높이가 증가하지 않는다. 이것이 Union-by-rank 최적화로, `rank=[]` 배열에 원소가 루트인 트리의 높이를 저장하고 union연산을 할 때 높이를 비교한 후 실행하도록 수정하면 된다.
이 과정으로 Find와 Union연산의 복잡도를 `O(logN)`까지 줄일 수 있다.

```py Union-by-rank 최적화
def union(a,b):
  a, b = find(a), find(b)
  if a==b:
    return -1
  if rank[a] > rank[b]: # 합치려는 두 트리의 높이를 비교
    a, b = b, a
  parent[a] = b
  if rank[a] == rank[b]: # 두 트리의 높이가 같은 경우에만 트리의 높이가 1 증가
    rank[b] += 1
```

### 경로 압축 Path compression

찾기 연산의 복잡도를 줄이기 위해, 이미 루트를 아는 원소는 그 루트를 저장하는 간단한 방법이 있다. 5 -> 3 -> 2 -> 1의 순서로 루트를 찾았다면, 5의 부모노드를 1로 갱신함으로써 다음에는 5 -> 1로 루트에 접근할 수 있게 된다.

```py path compression
def find(a):
  if a == parent[a]:
    return a
  else:
    parent[a] = find(parent[a]) # 부모노드를 루트로 갱신
    return parent[a]
```

두 최적화 방법을 모두 적용한 Union-Find 자료구조는, Find연산의 복잡도가 크게 감소한다. 특히 Find를 여러 번 수행할수록 빨라지게 되는데, 이 경우 평균적으로 `O(𝛼(N))`의 복잡도를 가진다고 한다. 𝛼(N)은 애커만 함수를 이용해 정의되는 함수인데 산술적인 N값에 대해서 그 값이 4 이하이기 때문에, 사실상 상수시간 내에 실행된다고 할 수 있다.

## BOJ 20040 사이클 게임

Path compression만 사용해도 제한 시간 내로 통과는 되는 것을 확인할 수 있었다. 입력이 많아질 수 있기 때문에 `sys.stdin.readline`을 이용해야 한다.

### 풀이

```python BOJ 20040 사이클게임
import sys
sys.setrecursionlimit(10**6)
input = sys.stdin.readline
n,m = map(int, input().split())
parent = [i for i in range(n)]
rank = [0 for i in range(n)]

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

def cycle():
  for x in range(m):
    a,b= map(int, input().split())
    if union(a,b) == -1:
      print(x+1)
      return
  print(0)

cycle()
```

> 참고문헌
> 프로그래밍 대회에서 배우는 알고리즘 문제해결전략 (종만북), 25장
