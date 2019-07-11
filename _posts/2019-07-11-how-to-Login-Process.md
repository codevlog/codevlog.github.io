---
layout: post
title: "[Howto] Firebase로 로그인 기능 활용하기"
author: "forget1026"
date: '2019-07-11 10:00:00 +0900'
categories: [howto, blog, markdown]
---

## Firebase를 이용한 Login방법 구현

### 목차
1. firebase key 받기
1. 로컬호스트에서의 회원가입
1. 로그인 구현
    1. Email을 이용한 구현
    1. google 아이디를 이용한 구현
    1. facebook 아이디를 이용한 구현
---

1. firebase key받기
![firebaseKey1](codevlog.github.io/_posts/img/Login/슬라이드1.png)

![firebaseKey2](_posts/img/Login/슬라이드2.png)

![firebaseKey3](/img/Login/슬라이드3.png)

![firebaseKey4](/img/Login/슬라이드4.png)

![firebaseKey5](/img/Login/슬라이드5.png)

![firebaseKey6](/img/Login/슬라이드6.png)

`TEST CODE`
```js
// console에 설치
// 1. npm init
// 2. npm install --save firebase //명령어로 firebase 설치

// 3. JavaScript에서 사용할 firebase 기본 설정 
import firebase from 'firebase/app'
import 'firebase/firestore'
import 'firebase/auth'

const POSTS = 'posts'
const PORTFOLIOS = 'portfolios'

// Setup Firebase
const config = {
	projectId: 'Your ProjectID',
	authDomain: 'Your authDomain',
	apiKey: 'Your apiKey',
	databaseURL: 'Your databaseURL',
	storageBucket: 'Your storageBucket'
}

firebase.initializeApp(config)
const firestore = firebase.firestore()
```
2. 로그인 구현
    1. Email을 이용한 구현

    `TEST CODE`
    ```js
    testFunction(email, password, name){
		firebase.auth().createUserWithEmailAndPassword(email, password) //firebase의 createUserWithEmailAndPassword함수를 사용
		.then(function(result) {
            //user.updateProfile 함수는 데이터를 더 넣고 싶을때 사용하시면 됩니다. 기본은 displayName과 photoURL 2개의 데이터가 들어갑니다.
            //error에서 어디서 error가 나왔는지 표시해줍니다. 각 함수의 error를 firebase의 문서에서 확인하셔야 합니다.
			result.user.updateProfile({ 
				displayName: name,
				photoURL: null
            }).catch(function(error) {
				console.log('[Update User Error]', error)
			})
		})
		.catch(function(error){
			console.log('[Create User Error]', error)
		})
	}
    ```
    2. Google로그인을 이용한 구현
        
    `TEST CODE`
    ```js
    testFunction() {
        let provider = new firebase.auth.GoogleAuthProvider() //GoogleAuth를 불러오기 위한 선언
        // firebase.auth().signInWithPopup()으로 할 경우 팝업창이 나오고 redirect로도 가능한 함수가 구현되어 있습니다.
        // 자세한 부분은 firebase 문서를 참조하시길 바랍니다.
		return firebase.auth().signInWithPopup(provider).then(function(result) {
			let accessToken = result.credential.accessToken
			let user = result.user
			return result
		}).catch(function(error) {
			console.error('[Google Login Error]', error)
		})
	}
    ```
    
    3. Facebook로그인을 이용한 구현
    ![firebaseKey7](/img/Login/슬라이드7.png)
    ![firebaseKey8](/img/Login/슬라이드8.png)
    ![firebaseKey9](/img/Login/슬라이드9.png)
    ![firebaseKey10](/img/Login/슬라이드10.png)
    ![firebaseKey11](/img/Login/슬라이드11.png)
    ![firebaseKey12](/img/Login/슬라이드12.png)
    
    `TEST CODE`
    ```js
    testFunction() {
        // 위의 Google Login과 유사함 
        let provider = new firebase.auth.FacebookAuthProvider();
		return firebase.auth().signInWithPopup(provider).then(function (result){
			let accessToken = result.credential.accessToken
			let user = result.user
			return result
		}).catch(function(error){
			console.error('[Facebook Login Error]', error)
		});
	},
    ```

## 참고문헌
firebase 문서 : https://firebase.google.com/docs/web/setup
facebook 문서 : https://developers.facebook.com/docs/