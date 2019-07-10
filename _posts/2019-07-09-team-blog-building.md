---
layout: post
title: "프로젝트 팀 블로그 Git Pages 이용해 만들기"
author: "coalee"
date: '2019-07-09 14:04:00 +0900's
summary: 팀블로그용 Git Pages 생성하기
category: howto
thumbnail: posts/hello.jpg
---

## 만드는 이유

 프로젝트를 기획하면서 우리 스스로의 니즈가 '우리가 배우는 것들을 잘 정리해보고 싶다'는 것이었다. 개발을 하면서 이곳저곳에서 검색하고 소스코드를 긁어다가 붙이고 하는 작업을 하게 되곤 하는데, 그렇게만 프로젝트를 완수하면 실제로 뭔가를 배웠다는 느낌은 들지 않는다. 배우고 구현하면서 알게 된 것들을 한번 정리해보자는 것에 서로 동의했고, 시작하게 됐다. 다른 사람들에게도 도움이 되면 좋겠다. 

 블로그 플랫폼은 쉽게 블로그를 만들어 공유할 수 있는 Git Pages를 활용하기로 했다. 다른 개발자들, 미래의 면접관 분들께도 더 어필할 수 있을거라는 생각도 있었다. 이것만 해도 처음 하다보니 은근 시간이 많이 소요됐다. 간략하게 정리해보려고 한다.



## 어떻게 만들까

#### 1. github organization 만들기

- github 블로그는 계정 당 하나만 만들수 있기 때문에 각 팀원 개인 계정이 아니라 Organization을 만들기로 했다. 
- 링크: https://github.com/organizations/new
- 이름을 적고(codev 이름이 이미 있어서 codevlog로 정함), 성격을 정하고 결제용 메일주소를 적으면 쉽게 만들 수 있다.



#### 2. Git Pages Repository 만들기

- Organization 계정에 Repository를 추가하고 이름을 `계정명.github.io`로 적는다. 우리의 경우 `codevlog.github.io`. 이렇게 이름을 정해야 git page로 등록이 된다.
- 내 컴퓨터로 `git clone https://github.com/Name.github.io` 한다.



#### 3. 테마 설정하기

- git pages는 jekyll을 활용해 마크다운으로 작성된 블로그 글을 포함한 파일 구조를 사이트 구조로 변환하는 방식이다.
- 미리 지정된 테마들이 많이 있어서 선택할 수 있다.
- 링크: http://jekyllthemes.org/
- 위 링크에 들어가서 마음에 드는걸 고르고(데모 페이지를 볼수 있는 것도 있다)
- 해당 repo에 들어가서 위에서 clone한 빈 repo에 내용물을 넣는다
- build하기(Windows10 기준). Rubygem bundler가 필요하다
  - 루비 설치: https://rubyinstaller.org/downloads/
  - Bundler 설치: `(cmd)$ gem install bundler`
  - git repo 폴더로 이동.
  - `(cmd)$ bundle install`
  - (선택) 로컬로 돌려보기: `(cmd)$ jekyll serve`



#### 4. 글 작성하기

- `_posts` 폴더에 `YYYY-MM-DD-title-of-post.md` 형식으로 마크다운 문서를 작성해 넣는다.
- `git add .`  &rarr; `git commit -m "메시지"` &rarr; `git push`
- 시간이 좀 지나고(아마 jekyll이 컴파일해서 업로드하는 시간), 서버에 반영이 된다
- `계정명.github.io` 접속

 

## 마치며

글 쓰는건 아무래도 시간이 생각보다 많이 걸리는 것 같다. 이왕 시간 들여서 만든 블로그 앞으로 잘 활용해보고 싶다. 