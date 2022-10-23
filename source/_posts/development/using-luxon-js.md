---
title: luxon.js 라이브러리로 날짜/시간 포맷팅하기
tags: []
categories: [Development]
permalink: ''
thumbnailImage: 'https://i.imgur.com/iwTskEa.png'
thumbnailImagePosition: right
date: 2022-10-23 23:35:03
---

<!-- excerpt -->

Backend에서 시간에 대한 정보가 담긴 데이터를 보내주는 유형은 다양하다. 일반적으로 js, ts에 존재하는 `Date` type으로 오거나, 한 번 처리해서 `202310232335`처럼 `string` type으로 오기도 한다. 또는 `10343904`같은 `number` type으로 오기도 하는데, 이 경우는 `1970.01.01 00:00:00 UTC`를 기준으로 지난 시간을 millisec의 단위로 표시한 것이다. (참고: [여기](https://currentmillis.com)에서는 현재 시간을 millisec 단위로 알려준다.)

Frontend 단에서 이 데이터를 `OOOO년 OO월 OO일 OO:OO` 같은 형식으로 바꿔줄 때, 지금까지는 `Date.getDate()`같은 함수를 이용해서 새로운 함수를 정의해서 썼었다. 그러나 luxon.js의 `DateTime`을 이용하면 포맷팅을 아주 쉽게 할 수 있다.

```ts
import { DateTime } from 'luxon';

DateTime.fromMillis(time * 1000).toFormat('yyyy.MM.dd  hh:mm');

// `toFormat()`함수를 이용해 내가 원하는 string 형식으로 변환할 수 있다.
```

아래 공식문서를 참고해보면 이 외에도 유용한 기능이 많다.

##

- [npmjs](https://www.npmjs.com/package/luxon)
- [Github Docs](https://moment.github.io/luxon/api-docs/index.html)
