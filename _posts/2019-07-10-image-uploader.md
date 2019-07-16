---
layout: post
title: "[개발일지] Image Uploader"
author: "coalee"
date: '2019-07-10 16:00:00 +0900
categories: [develop, frontend, component, vue]
---

## Motivation

- 글을 작성할 때, 이미지 파일을 업로드하는 일이 필요하다.
- 글 뿐만 아니라 포트폴리오에 내용을 올릴 때에도 필요하므로 Vue 컴포넌트로 만들어 재활용이 가능하도록 한다.
- 어떤 서버에 어떻게 업로드할지 장단점을 고려해 결정해야 한다.



## Develop Plan

1. 클라이언트에 사진 첨부 버튼 만들고 미리보기가 뜨도록 한다
2. 얻은 경로를 API를 이용해 어딘가의 서버로 업로드한다
   - 파일 경로를 가지고 어떻게 데이터를 전송할 수 있는지 알자
   - 서버: Imgur API 이용, Firebase 이용, 자체 개발 서버(스프링) 이용
3. 업로드한 주소를 가지고 포트폴리오 화면에 띄워줄 수 있도록 한다



## Learning & Implementation

#### Imgur API

1. 회원가입
2. OAuth2
   1. Registration:
   2. Authorization: Postman을 활용해서
   3. 요청
3. API Request



## Result





## 참고사이트 

- [vuetify image 컴포넌트](https://vuetifyjs.com/en/components/images)
- [Tutorial: how to build image uploader component](https://www.freecodecamp.org/news/how-to-build-a-flexible-image-uploader-component-using-vue-js-2-0-5ee7fc77516/)



