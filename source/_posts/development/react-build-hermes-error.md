---
title: "[React] Library not loaded: @rpath/hermes.framework/hermes 에러"
tags: [React]
categories: [Development]
permalink: ""
thumbnailImage: ""
thumbnailImagePosition: right
date: 2022-11-21 21:29:08
---

<!-- excerpt -->

React Native 0.70.0 이상으로 업데이트 하면서 새로운 프로젝트 빌드 시에 build success 후 시뮬레이터에서 튕기는 오류를 겪었다. 해결 방법은 아래와 같다.

<br />

{% image fancybox clear center fig-100 https://i.imgur.com/zwdiQV5.png %}

xcode > build phases > Link binary with Libraries 메뉴에 hermes.xcframework 추가

<br />

{% image fancybox clear center fig-100 https://i.imgur.com/ksgRFIN.png %}

xcode > General > Frameworks, Libraries, and Embedded Content 에 hermes.xcframework 추가 후 Embed & Sign

> [Github - Library not loaded: @rpath/hermes.framework/hermes on iOS #34601]('https://github.com/facebook/react-native/issues/34601')
