---
layout: post
title: "OpenweathermapAPI 사용하기 (feat.Vue)"
author: "Geon._.Uk"
date: '2019-07-11 09:33:00 +0900'
categories: [Vue, Javascript, API]
---

## Goal

```markdown
 - OpenweathermapAPI를 활용하자
 - 받은 데이터를 웹페이지에 Vue로 뿌려주자
```
## 0. Basic Setup

- Default Install
>- npm install
>- npm install -g yarn
>- npm install -g @vue/cli
>- npm install vue

- Install axios
>- npm install --save axios

- Make vue page by cli
>- vue init webpack-simple (prjectName)

## 1. OpenweathermapAPI

- weather API는 https://openweathermap.org/ 에서 데이터를 받아 올 예정이다.
- 데이터를 받기위해 API Key가 필요하기 때문에 회원가입후 로그인을 한다.
- 로그인 뒤, API Key를 발급 받는다.

완성한 Vue Page를 실행하면 흔한 Vue 시작 페이지가 보인다.
거기서 Vue 이미지를 서울의 날씨 이미지로 설정하고,
날씨와 온도(화씨) 습도를 밑에 적어보자.

```javascript
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
  </div>
</template>
<script>
    export default {
        name: 'app',
        data() {
            return {
                msg: 'Welcome to Your Vue.js App'
            }
        }    
    }
</script>
```

처럼 style부분을 제외하고 필요없는 부분은 전부 삭제를 했다.

받은 API Key를 'a'라고 정한뒤 (지역은 서울이기 때문에 서울로 했다, 만약 다른 지역을 원한다면 Seoul을 다른 지역으로 바꾸어준다.)
http://api.openweathermap.org/data/2.5/weather?q=Seoul&appid=(APIKey)
에다가 잘 넣어준다면,

![weatherData](img/openweatherAPI/weatherData.png)
식의 데이터를 볼 수 있다.

script 태그 안에

```javascript
import axios from 'axios'
axios.defaults.baseURL = 'http://api.openweathermap.org/data/2.5'

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      weatherIconUrl:'',
      weatherDescription:'',
      temp:null,
      humidity:null
    }
  },
  methods:{
    showWeather(){
      axios
      .get('/weather?q=Seoul&appid=a')
      .then(response=>{
        this.weatherIconUrl = "http://openweathermap.org/img/w/"+response.data.weather[0].icon+".png";
        this.weatherDescription = response.data.weather[0].description;
        this.temp = response.data.main.temp;
        this.humidity=response.data.main.humidity;
      })
    }
  },
  created(){
    this.showWeather()
  }

```

의 소스를 입력한다. Vue의 생명주기인 created 에서 showWeather 메소드를 실행시킨다.
이제 template을 수정하자

```html
<template>
  <div id="app">
    <img :src="weatherIconUrl">
    <h1>{{ msg }}</h1>
    <h2>{{weatherDescription}}</h2>
    <h2>{{temp}}</h2>
    <h2>{{humidity}}</h2>
  </div>
</template>
```

위의 소스를 작성 후 실행을 하면,

## 2. Result
![result](img/openweatherAPI/result.png)
의 결과가 나타난다.

## 3. 참고사이트

https://blog-han.tistory.com/35
https://devtimothy.tistory.com/91
