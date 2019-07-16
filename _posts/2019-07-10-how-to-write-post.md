---
layout: post
title: "[Howto] 포스트 작성하는 법"
author: "coalee"
date: '2019-07-10 10:00:00 +0900
categories: howto
---

## 먼저, 포스트 작성의 이유와 이점

- 작업하는 내용을 정리하면서 논리적인 구조로 문제에 접근할 수 있다
- 글로 표현하는 과정을 통해 누군가에게 설명할 수 있는 수준으로 향상
- 다른 팀원들이 어떤 작업을 했는지 쉽게 이해할 수 있다
- 본인이 나중에 다시 참고할 때 도움이 될 것이다
- 개발자의 주요 역량 중 하나인 문서 작성 능력 향상!

#### *** 주의**: 문서 작성을 완벽하게 할 필요는 없습니다. 조금은 거칠어도 알아볼 수 있을 정도의 수준으로 



## 작성법 요약

- **깃 블로그**: Git Pages 서비스를 활용해 블로그 형식으로 글을 올리는 것. 
  - 다른 형식으로 운영하는 모습은 아직 보진 못했다. Git Pages == 블로그?
- **Git Pages**: Github repo에 올라와있는 파일 구조를 웹페이지로 변환해주는 것
- **포스팅**: Github repo에 정해진 규칙에 맞춰 마크다운 문서를 작성하고, commit하는 것
- **정해진 규칙**: `_posts` 폴더 아래에 파일명 `YYYY-MM-DD-some-subject-title-blah.md`로 작성. (예: `2019-07-09-team-blog-building.md`)



## How-to + 문서 규격

```
---
layout: post
title: "[Howto] 포스트 작성하는 법"
author: "coalee"
date: '2019-07-10 10:00:00 +0900'
categories: [howto, blog, markdown]
---
## 소제목1
본문 본문


## 소제목2


## 참고사이트
- [Git Pages Help](https://help.github.com/en/categories/customizing-github-pages)
- [구글](https://google.com)
```

- 문서 맨 위 `---` 사이에 글에 대한 metadata를 작성한다. 
- 아래에 본문을 작성한다. 소제목부터 ## 두개 붙여서 작성하시면 됩니다.
- 본문은 자신의 스타일에 따라서 자유롭게
- 마지막에 참고사이트를 적어주면 다른 사람들과 미래의 나에게 도움이 됩니다 :)



## 참고사이트

- [Jekyll Post 작성하기](https://jekyllrb-ko.github.io/docs/posts/)

