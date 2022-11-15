---
title: React-Query를 알아보자
tags: [React]
categories: [Development]
permalink: ''
thumbnailImage: ''
thumbnailImagePosition: right
date: 2022-10-31 22:03:24
---

How to use React-Query

<!-- excerpt -->

<!-- toc -->

이번 프로젝트에서 상태관리 및 서버 연동을 위해 `react-query` 라이브러리를 처음 써봤다. 프론트 단에서 refetch를 하거나 상태를 관리할때 아주 유용해서 앞으로도 계속 쓸 것 같다.

## 사용방법

### 설치

```bash
yarn i react-query
```

### App.tsx

```ts
const queryClient = new QueryClient();
```

`<QueryClientProvider />`로 메인 컴포넌트를 감싸주어야 한다.

## useQuery

> A query is a declarative dependency on an asynchronous source of data that is tied to a unique key

`useQuery`는 서버측에 get 요청을 보내서 받아온 데이터를 가공하고 관리한다. 만약 서버측 데이터를 바꿔야 한다면(post, put, delete method) `useMutation`을 사용해야 한다.

특정 쿼리를 사용(subscribe)하려면, `useQuery`훅에 다음 두 파라미터가 필요하다.

- 그 쿼리의 `uniqueKey`
- 비동기 api 호출 함수

```ts
const info = useQuery('todos', fetchTodoList);
```

useMutation
useQuery

> 참고
> React Query 아주 잘 설명된 블로그
> https://kyounghwan01.github.io/blog/React/react-query/basic/
