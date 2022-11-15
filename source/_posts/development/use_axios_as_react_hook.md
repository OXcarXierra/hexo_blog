---
title: '[React] Axios를 React Hook으로 사용하기'
tags: [React]
categories: [Development]
permalink: ''
thumbnailImage: ''
thumbnailImagePosition: right
date: 2022-11-05 22:03:24
---

useAxios()

<!-- excerpt -->

<!-- toc -->

그동안은 axios를 그냥 import해서 사용했는데, 이렇게 `useAxios()` 커스텀 훅을 만들어서 사용하면 에러 처리나 baseUrl에 대한 전처리를 쉽게 해줄 수 있다.

```ts useAxios.ts
import extra from '@utils/extra';
import axios, { AxiosError } from 'axios';
import { useAtomValue } from 'jotai';
import { useMemo } from 'react';

const useAxios = () => {
  const user = useAtomValue(userAtom);
  const axiosInstance = useMemo(() => {
    const instance = axios.create({
      baseURL: yourBackendBaseURL,
    }); // Axios Instance 생성
    instance.interceptors.request.use(async (config) => {
      const token = await yourToken; // header에 들어갈 token 받아오기
      return {
        ...config,
        headers: {
          ...(config.headers ?? {}),
          Authorization: token,
        },
      };
    });
    instance.interceptors.response.use(undefined, async (value) => {
      if (value instanceof AxiosError) {
        console.error(
          `AxiosError(${value.response?.status}/${value.code}): ${value.message}\n${value.response?.data}`
        ); // 에러 출력
      } else {
        console.log(value.response);
      }
      return value;
    });
    return instance;
  }, [user]);
  return axiosInstance;
};

export default useAxios;
```
