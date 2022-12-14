---
title: 비트마스킹 Bitmask
tags: [data structure]
categories: [Algorithm]
thumbnailImage: https://i.imgur.com/jOe1hdP.png
thumbnailImagePosition: right
permalink: ''
date: 2022-08-22 22:03:21
---

**BOJ 1052번 물병**

<!-- excerpt -->
<!--toc-->

## 비트마스킹이란?

**이진법 숫자의 각 자리(비트)에** `true`/`false` **혹은 0/1의 데이터를 저장**하는 자료구조 혹은 테크닉이다.
예를 들어, 0,1,2,3중 두 숫자를 뽑는 모든 경우의 수를 생각하자. `[0,1], [0,2], [0,3], [1,2], [1,3], [2,3]`처럼 행렬로 묶는 방법이 있다.
비트마스킹을 이용하여 i의 포함 여부를 2^i자리에 저장한다면, `0011 0101 0110 1001 1010 1100`로 여섯 가지 방법을 나타낼 수 있다. 이 숫자들을 십진법으로 바꾸면 각각 `3,5,6,9,11,13`이 된다.
즉, 리스트형 데이터를 정수형으로 간단하게 표현할 수 있게 된 것이다. 이렇게 비트마스킹은 원소들의 집합을 표현할 때 유용하다.

### 비트 연산자

이렇게 만들어진 이진수(혹은 정수)는 아래 5가지의 연산이 가능하다.

- **AND**
  입력받은 두 이진수의 각 자리수가 모두 1일 때만 결과에도 1을 반환
  `1100 ^ 1010 = 1000`
- **OR**
  입력받은 두 이진수의 각 자리수 중 하나라도 1일 때 결과에도 1을 반환
  `1100 ^ 1010 = 1110`
- **XOR**
  입력받은 두 이진수의 각 자리수 중 하나만 1일 때 결과에도 1을 반환
  `1100 ^ 1010 = 0110`
- **NOT**
  입력받은 이진수의 비트가 0일 때 1, 1일 때 0을 반환
  `~1100 = 0011`
- **shift**
  이진수의 비트를 왼쪽, 오른쪽으로 원하는 만큼 움직인 값을 반환
  `11 << 2 = 1100`
  `1100 >> 2 = 11`

### 비트 연산자를 이용한 데이터 제어

- **비트 삽입**
  p번 비트 추가 : `x = x | (1<<p)`
  `1010 | (1<<2) = 1010 | 100 = 1110`
- **비트 삭제**
  p번 비트 삭제 : `x = x & ~(1<<p)`
  `1110 & ~ (1<<2) = 1110 & 1011 = 1010`
- **비트 조회**
  p번 비트 조회 : `x & (1<<p)`의 p번 비트
  `1110 & (1<<2) = 1110 & 0100 = 0100`
- **비트 토글**
  p번 비트 토글 : `x = x ^ (1<<p)`
  `1110 ^ (1<<2) = 1110 ^ 0100 = 1010`

## BOJ 1052번 물병

### 문제

[BOJ 1052번 물병]('https://www.acmicpc.net/problem/1052')

### 풀이

문제를 수기로 조금 풀어보면, 물병의 물을 합치다 보면 `2^i`리터들의 합으로 분리됨을 알 수 있다. 즉 총 물의 양인 `N`을 이진수로 표현했을 때 1의 개수만큼의 비지 않은 병이 생긴다. 결국, `N+cnt`를 이진수로 표현했을 때 1의 개수가 k보다 작거나 같도록 하는 최소의 `cnt`를 구하는 문제가 된다.
`cnt`를 1씩 증가시키면서 계산하면 최악의 경우 복잡도가 `O(NlogN)`이라서 시간초과가 난다. 1이 있는 가장 작은 자리수부터 1을 추가해주면 계산이 간단해진다.

> int형 자료 N의 이진법 표현은 `bin(N)`으로 구할 수 있으며, `0b`로 시작하기 때문에 `bin(N)[2:]`를 사용하기도 한다.

```py BOJ 1052번 물병
n, k = map(int, input().split())
cnt = 0
while bin(n).count('1') > k:
  b = bin(n)
  idx = len(b)-b.rfind('1')-1
  n += (1 << idx)
  cnt += (1 << idx)
print(cnt)
```

> 참고자료
> 프로그래밍 대회에서 배우는 알고리즘 문제해결전략 (종만북), 16장
