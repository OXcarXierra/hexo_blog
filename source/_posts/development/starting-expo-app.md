---
title: '[React] Expo 이용해서 React Native 앱 시작하기'
tags: [React]
categories: [Development]
permalink: ''
thumbnailImage: 'https://i.imgur.com/pMDZp1L.png'
thumbnailImagePosition: right
date: 2022-09-27 11:20:51
---

Starting React Native app with Expo

<!--excerpt-->

## Expo CLI 설치

```bash
npx expo whoami # 로그인된 계정 확인, Not logged in 이 뜨면 로그인해줘야함
npx expo register # 회원가입
npx expo login
```

## Typescript - Expo 앱 시작

```bash
npx create-expo-app app-name
cd app-name
touch tsconfig.json
npx expo start # Typescript 필요 번들설치
mv App.js App.tsx
npx tsc # 타입체크
```

또는, 이미 만들어진 Typescript 기반의 expo 템플릿을 설치하여 사용할 수도 있다.

```bash
npx create-expo-app -t expo-template-blank-typescript
```

> 참고자료 : [Expo Docs]('https://docs.expo.dev/guides/typescript/')
