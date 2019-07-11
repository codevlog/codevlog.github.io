---
layout: post
title: "Github API 사용하기 (feat.Vue)"
author: "Geon._.Uk"
date: '2019-07-11 17:02:00 +0900'
categories: [Vue, Javascript, API, Git]
---

## Goal

```markdown
 - Github API를 활용하자
 - 받은 데이터를 웹페이지에 Vue로 뿌려주자
```

## 0. Basic Setup

- Default Install
>- npm install
>- npm install -g yarn
>- npm install -g @vue/cli
>- npm install vue
>- npm install vuetify

- Install axios
>- npm install --save axios

- Install axios
>- npm install --save axios

- Make vue page by cli
>- vue create ((projectName))

- make api.js
- make GithubService.js

vuetify를 사용할 것이기 때문에,
main.js에

```javascript
import Vuetify from 'vuetify'
import 'vuetify/dist/vuetify.min.css'
import App from './App.vue'

Vue.use(Vuetify)
```
를 추가해 주자.

## 1. Github or Gitlab API

- github를 주로 사용하기 때문에 github에서 repository 데이터를 받아 올 예정이다.
- 데이터는 public repository만 볼 수 있다.

완성한 Vue Page를 실행하면 흔한 Vue 시작 페이지가 보인다.
거기서 Vue 마크 밑에 나의 repository를 출력해 보자.

```javascript
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>My Repository</h2>
    
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```
(vuetify를 사용할 거고, 따로 style은 건들 필요가 없어 생략했다.)

이제 github에서 제공해주는 API를 사용하기위해 제공 문서를 읽어보자.
https://developer.github.com/v3/

By default, all requests to https://api.github.com 라고 첫줄에 적혀있기 때문에 BASE_URL으로 정한다.

이후 https://developer.github.com/v3/projects/#list-user-projects 에서 잘 찾아보면 

List user pojects 란에서

> GET 방식의 /users/:username/projects 라는 Url을 받을 수 있다.
username에 본인의 git name을 잘 적어주자. 나는 parkgunuk 라는 username이다.

그래서 https://api.github.com/users/parkgunuk/projects 를 인터넷에서 접속하면 JSON 형식의 데이터가 주어진다.

여기서 repository 의 full_name을 가져올 예정이다.

- api.js
```javascript
import axios from 'axios'

export default (baseURL) => {
	return axios.create({
		baseURL: baseURL,
		withCredentials: false,
		headers: {
			'Accept': 'application/json',
			'Content-Type': 'application/json'
		}
	})
}
```
- GithubService.js
```javascript
import api from './api'

const BASE_URL = 'https://api.github.com'

export default {
	getRepos(userName) {
		return api(BASE_URL).get(`/users/${userName}/repos`)
	},
	getCommits(userName, repo) {
		let d = new Date()
		d.setYear(d.getYear() - 1)
        
        return api(BASE_URL).get(`/repos/${userName}/${repo}/commits?since=${d.toISOString()}`)
	}
}
```
와 같이 소스를 작성해 준다.

이제 App.vue에서 뿌려주기 위한 소스코드 작업을 시작하자.

App.vue template 부분
```javascript
<div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
    <h2>My Repository</h2>
    <v-layout column px-4>
        <v-flex v-for="(i,idx) in repositories">
            <v-divider v-if="idx === 0"></v-divider>
                <h2>{{i.full_name}}</h2>
            <v-divider></v-divider>
        </v-flex>
    </v-layout>
</div>
```
App.vue script 부분

```javascript
    import GithubService from './GithubService'
    import Repository from './Repository'
        export default {
        name: 'app',
        data () {
            return {
            msg: 'Welcome to Your Vue.js App',
            repositories:[]
            }
        },
        components: {
            Repository
        },
        mounted(){
            this.getGithubRepos('parkgunuk')
        },
        methods:{
            async getGithubRepos(userName) {
                    const response = await GithubService.getRepos(userName)
                    if(response.status !== 200) {
                        return
                    }
            this.repositories = response.data
                }
        }
    }
```
## 2. Result
    
![result](https://codevlog.github.io/assets/img/githubAPI/result.PNG)
실행결과 별로 없는 repo들이 아주 잘 나왔다.
## 3. 참고사이트

- https://developer.github.com/v3/
