---
title: 최장 공통 부분 수열 알고리즘
tags: [dp, lcs]
categories: [Algorithm]
thumbnailImage: https://i.imgur.com/ydX4P1X.png
thumbnailImagePosition: right
permalink: ''
date: 2022-07-25 15:48:40
---

**BOJ 9251 LCS, BOJ 9252 LCS 2**

<!-- excerpt -->
<!-- toc -->

## Longest Common Substring, 최장 공통 문자열

Substring인 경우 최장 공통 문자열을 찾는 다른 문제가 된다.
<br>

- 2차원 배열을 두어서 A[i]와 B의 모든 문자열을 검토.
- A[i]와 B[j]가 같다면, LCS[i][j] = LCS[i-1][j-1]+1
- 다르다면, LCS[i][j] = 0
- LCS중 최댓값

## Longest Common Subsequence, 최장 공통 부분 수열

Substring의 경우와 매우 비슷하지만, A[i]와 B[j]가 다를 경우의 취급이 다르다.
<br>

- A[i]와 B[j]가 같다면, LCS[i][j] = LCS[i-1][j-1]+1
- 다르다면, LCS[i][j] = max(LCS[i-1][j] ,LCS[i][j-1])
  그 이유는.. 이해하기 오래 걸렸지만 생각해보면 당연하다.
  A[i-1]와 B[j-1]까지 비교한 상황에서 A[i]가 추가되었을 때 새롭게 늘어나는 subsequence와 B[j]의 그것을 비교해주는 논리이다.
- 모두 완성한 LCS의 마지막 원소

완성한 LCS의 표를 마지막 원소로부터 거꾸로 올라가면 만족하는 subsequence를 얻어낼 수도 있다. 이는 BOJ 9252 LCS2 문제이기도 하다.
<br>

- 완성한 LCS행렬의 마지막 원소에서 출발한다.
- 해당 원소의 왼쪽이나 위쪽(LCS[i-1][j] or LCS[i][j-1])에 같은 값이 있다면, 그쪽으로 이동한다. (나는 왼쪽부터 탐색하도록 했다.)
- 없다면, LCS행렬을 완성할 때 A[i]와 B[j]가 같아서 값이 증가한 값이라고 볼 수 있다. 왼쪽대각선(LCS[i-1][j-1])으로 이동한다.
- 이동하면서 해당 위치의 알파벳을 다른 행렬 Sequence의 끝에 저장한다.
- i나 j가 0이 되었을 경우, Sequence를 뒤집으면 최장 공통 부분 수열을 얻을 수 있다.

## BOJ 9251 LCS

### 문제

LCS(Longest Common Subsequence, 최장 공통 부분 수열)문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다.

예를 들어, ACAYKP와 CAPCAK의 LCS는 ACAK가 된다.

### 풀이

```python
A = input()
B = input()
n, m = len(A),len(B)

LCS = [[0]*(m+1)for _ in range(n+1)]
for i in range(1,m+1):
  for j in range(1, n+1):
    if A[j-1] == B[i-1]:
      LCS[j][i] = LCS[j-1][i-1] + 1
    else:
      LCS[j][i] = max(LCS[j-1][i], LCS[j][i-1])
print(LCS[n][m])
```

## BOJ 9252 LCS 2

### 문제

LCS(Longest Common Subsequence, 최장 공통 부분 수열)문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다.

예를 들어, ACAYKP와 CAPCAK의 LCS는 ACAK가 된다.

### 풀이

```python BOJ 9252
a = input()
b = input()
la ,lb = len(a), len(b)
LCS = [[0]*(lb+1) for _ in range(la+1)]

for i in range(1, la+1):
  for j in range(1, lb+1):
    if a[i-1] == b[j-1]:
      LCS[i][j] = LCS[i-1][j-1]+1
    else:
      LCS[i][j] = max(LCS[i-1][j], LCS[i][j-1])

sequence = []
x,y = la, lb
while x!=0 and y!= 0:
  if LCS[x][y] == LCS[x-1][y]:
    x -= 1
  elif LCS[x][y] == LCS[x][y-1]:
    y -= 1
  else:
    sequence.append(a[x-1])
    x-=1
    y -= 1

print(LCS[la][lb])
print(*reversed(sequence), sep='')
```

> 참고자료
> https://velog.io/@emplam27/알고리즘-그림으로-알아보는-LCS-알고리즘-Longest-Common-Substring와-Longest-Common-Subsequence
