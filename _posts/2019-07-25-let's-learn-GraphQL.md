---
layout: post
title: "GraphQL에 대한 얕은 물"
author: "Geon._.Uk"
date: '2019-07-24 18:48:00 +0900'
categories: [GraphQL, API, Git, Language]
---

## Goal

```markdown
 - Github API 에서 GraphQL을 사용해 데이터를 받아보자
```

## 0. Basic Setup

```
GraphQL을 사용해 데이터를 받아오기 위해,
여러가지 방법중 GraphQL PlayGround를 사용하고자 한다.

그래서 GraphQL PlayGround를 설치를 해주면 된다.
```

## 1. Theory

GraphQL은 유우우우명한 Facebook에서 Server API를 구성하기위해 만든 Query Language이다. Server API를 구성하기 위한 언어이다.
Server API를 구성하다 보니 주로 RESTful API와 비교 하면서 장단점과, 예제를 통해 Github API에서 데이터를 추출해보기로 하자.

#### 1) 왜 FaceBook은 Query Language를 만들었는가?
[GraphQL 블로그](https://graphql.org/blog/graphql-a-query-language/)에서 글을 읽어보면 iOS와 Android 등 다양한 기종에서 필요한 정보들을 일일히 세밀하게 구현하기 힘들어 표준화 된 Query Language를 만들었다고 한다.

#### 2) GraphQL의 장점
사용하면서 느낀 가장 큰 장점은 원하는 데이터를 JSON 형식 비슷하게 보내면 원하는 결과를 맞춰서 Return 한다는 점이다. 일반적인 RESTful API는 여러 URL에서 데이터를 받아와야 하지만 GraphQL API는 한번의 요청으로 앱에 필요한 모든 데이터를 가져온다는 것이 가장 큰 장점으로 다가왔다.

그림으로 설명하면 다음과 같다.
아래는 일반적인 RESTful 의 구조이다.
![restful](/assets/img/graphQL/restful.png)
하지만 GraphQL의 구조는 조금 다르다.
![GraphQL](/assets/img/graphQL/graphql.png)

위의 두 구조를 살펴보면 RESTful 같은 경우는 URL 마다 마다 데이터를 받아야하기 때문에 접속요청이 증가한다. 하지만 GraphQL은 한번에 데이터를 모아 가져오기 때문에 요청횟수가 줄어들게 된다.

#### 3) GraphQL의 단점

장점만 나열했지만 GraphQL의 단점 또한 존재한다.
- 첫 번째 재귀적인 요청이 불가능하다.
- 두 번째 요청과 응답이 정해진 endpoint가 존재하는 경우 RESTful에 비해 요청의 크기가 크다.
- 세 번째 Text만으로 전달하기 힘든 내용을 처리하기 귀찮다.

#### 4) 정리

RESTful과 GraphQL마다 상황에 맞게 혼용해서 쓰면 될 듯 싶다.

## 2. Example
github API에서 나의 Repository를 3개만 가져오는 GraphQL를 한번 구현해보자.


- github API 제공문서에 따르면 개인의 token이 필요하다고 하므로 본인 token 값이 필요하다.

본인 token을 발급 받았다면 GraphQL Playground를 실행하고..

- url을 입력하는 곳에 github API를 사용할 예정이므로.. https://api.github.com/graphql 를 입력한다.
- 좌측 하단 HTTP HEADER를 클릭하고,
  ```GraphQL
  {
    "Authorization" : "bearer (your token)"
  }
  ```
  를 입력 해준다.

- 그다음 필요한 데이터 query문을 입력한다. 아무거나 입력한다고 값이 return 되는 것은 아니기 때문에 github API 문서를 참조하며 적길 바란다. 나는 나의 name, url, repository 3개만 출력 하려한다.
  ```GraphQL
  query{
    viewer {
      name,
      url,
      repositories(first: 3) {
        edges {
          node {
            name,
          }
          cursor
        }
      }
    }
  }
  ```

위와 같은 소스를 실행해 return 받는 데이터를 살펴보면 다음과 같다.
```JSON
{
  "data": {
    "viewer": {
      "name": "Geon._.Uk",
      "url": "https://github.com/parkgunuk",
      "repositories": {
        "edges": [
          {
            "node": {
              "name": "ClassificationActivity"
            },
            "cursor": "Y3Vyc29yOnYyOpHOBIkjxg=="
          },
          {
            "node": {
              "name": "DataStructureStudy"
            },
            "cursor": "Y3Vyc29yOnYyOpHOB2mHuw=="
          },
          {
            "node": {
              "name": "ML_iOS_exam"
            },
            "cursor": "Y3Vyc29yOnYyOpHOCIpkJQ=="
          }
        ]
      }
    }
  }
}
```
## 3. 참고
- https://graphql.org/blog/graphql-a-query-language/
- https://www.holaxprogramming.com/2018/01/20/graphql-vs-restful-api/
