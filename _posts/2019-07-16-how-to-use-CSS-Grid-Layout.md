---

layout: post

title:  "CSS Grid Layout 이해하기"

author: "nenemttin"

date: '2019-07-16 12:50 +0900'

categories: [css, grid, guide]
---





## 필요성

* Vuetify, bootstrap의 그리드 시스템은 확실히 강력하지만 활용할 수 없는 상황에 대비하기 위해서 공부해보자





## Important Terminology

* **Grid Container**

  ![1563179808259](/assets/img/Grid/img1.png)

  * display: grid 하면 적용된다.
  * 모든 grid 아이템들의 부모가 된다.





* **Grid Item**

  ![1563179972366](/assets/img/Grid/img2.png)

  * grid container의 자식들
  * 여기서 item 요소들은 grid 아이템이 맞지만, sub-item은 아니다. 





* **Grid Line**

  ![1563180333092](/assets/img/Grid/img3.png)

  * row grid lines 와 colum grid lines  두 종류가 있다.
  * 범위 지정할 때 사용





* **Grid Track**

  ![1563180482951](/assets/img/Grid/img4.png)

  * 인접한 두 grid lines 사이의 공간을 의미.
  * 위 사진은 2번과 3번 row lines 사이의 grid track





* **Grid Cell**

  ![1563180588234](/assets/img/Grid/img5.png)

  * grid의 한 unit이다.





* **Grid Area**

  ![1563180724119](/assets/img/Grid/img6.png)

  * 4개의 grid lines들로 둘러 쌓인 공간





## Grid Properties Table of Contents

### Properties for the Parent (Grid Container)



* **display**

  ![1563235330445](/assets/img/Grid/img7.png)

  * Grid container 선언
  * grid - block level grid를 생성
  * inline-grid - inline level grid 생성





* **grid-template-columns/rows**

  ![1563235792802](/assets/img/Grid/img8.png)

  * grid의 column 과 row 값 설정. 값은 track size나 grid line 사이의 공간을 의미(크기)
  * 값을 설정하는 방식이 다양하다 
    * row, column의 숫자를 활용 ([row1-start] 25% [row1-end])
    * 중복되는 값이 반복될 때 repeat() 활용 (repeat(3, 20px [col-start]))
    * fr(fraction of available space) unit 사용 (1fr 1fr 1fr = 1/3 씩 사용하겠다), free space는 non-flexible 한 아이템들을 할당한 후에 계산됨





* **grid-template-areas**

  ![1563238555289](/assets/img/Grid/img9.png)

  * grid-area 속성으로 특정된 grid area의 이름을 참조해서 grid template을 정의
  * 선언 시 area 이름을 반복하면 영역 확장. `.`은 빈 셀을 의미한다.
  * 예시

  ![1563238773901](/assets/img/Grid/img10.png)

  * 각 행에 대해서 선언을 할 때 같은 셀 수를 가지고 있어야함
  * area에 대해 이름을 부여하면 자동적으로 line 이름이 생김. (name-start / name-end), line은 여러 이름을 가질 수 있다.





* **grid-template**

  ![1563239147554](/assets/img/Grid/img11.png)

  * 앞선 grid-template-rows / grid-template-columns / grid-template-area를 한번에 설정할 수 있는 방법







* **grid-column-gap / grid-row-gap**

  ![1563239446614](/assets/img/Grid/img12.png)

  * grid line들의 size를 설정
  * 영역 안 쪽만 설정되며, 경계는 제외.
  * 예시

  ![1563239645004](/assets/img/Grid/img13.png)





* **grid-gap**

  ![1563239729385](/assets/img/Grid/img14.png)

  * grid-row-gap / grid-column-gap 한 번에 설정하는 방법





* **justify-items**

  ![1563239972341](/assets/img/Grid/img15.png)

  * grid 아이템들을 행에 정렬하는 방식
  * **start** - 시작 / **end** - 끝 / **center** - 중간 / **stretch** - 꽉 차게(default)
  * 예시

  ![1563240418211](/assets/img/Grid/img16.png)







* **align-items**

  ![1563240537687](/assets/img/Grid/img17.png)

  * justify-items의 세로 형식
  * 예시

  ![1563240611895](/assets/img/Grid/img18.png)







* **place-items**
  * align-items 와 justify-items를 한 번에 설정하는 방법







* **justify-content**

  * grid container 크기에 비해 grid area 크기의 총 합이 적을 때 그 안에 content를 정렬하는 방법
  * **value**
    * **start** - 시작 지점에 정렬
    * **end** - 끝 지점에 정렬
    * **center** - 중간 지점에 정렬
    * **stretch** - container 꽉 차게 정렬
    * **space-around** - 각각 grid 아이템들 사이의 공간을 동일하게 주고 양 끝은 그 공간의 절반을 할당
    * **space-between** - 각각 grid 아이템들 사이의 공간을 동일하게 주고 양 끝 공간 없음
    * **space-evenly** - grid 아이템들과 양 끝 공간을 동일하게 할당
  * 예시

  ![1563241141150](/assets/img/Grid/img19.png)







* **align-content**

  * justify-content 세로 형식

  ![1563241299363](/assets/img/Grid/img20.png)

  * 예시

  ![1563241351139](/assets/img/Grid/img21.png)







* **place-content**
  * align-content 와 justify-content를 한 번에 설정하는 방법







* **grid-auto-columns / grid-auto-rows**

  * grid cell보다 더 큰 아이템이 있을 때나 기존 grid의 바깥 쪽에 grid 아이템이 놓일 때 자동으로 지정된 size의 grid track을 생성해준다.
  * 예시

  ![1563242990354](/assets/img/Grid/img22.png)







* **grid-auto-flow**

  * grid 아이템들을 명확하게 위치를 지정해서 배치하지 않으면, 자동 위치 지정 알고리즘이 발동해 자동으로 아이템들을 배치한다. 이 속성은 위치 지정 알고리즘을 작동하는 방식을 제어한다.
  * **Value**
    * **row ** -   행 우선으로 각 행에 차례로 아이템들을 채우며, 필요하면 행을 추가한다(default)
    * **column** - 열 우선으로 각 열에 차례로 아이템들을 채우며, 필요하면 열을 추가한다.
    * **dense** - 작은 크기의 아이템이 나중에 나타날 경우 빈 공간에 채운다.
  * 예시

  ![1563243875363](/assets/img/Grid/img23.png)







* **grid**

  * 앞서 소개한 grid-template-rows/columns, grid-template-areas, grid-auto-rows/columns, grid-auto-flow 속성들을 한 번에 설정 가능
  * 예시(아래 두 그림은 같은 설정이다.)

  ![1563244014904](/assets/img/Grid/img24.png)











### Properties for the Children(Grid Items)



* **grid-column-start / grid-column-end / grid-row-start / grid-row-end**

  * 특정 grid line을 이용해서 grid 아이템의 위치를 정한다.
  * **Value**
    * line - line의 숫자 입력 또는 line의 이름
    * span number 
    * span name 
    * auto 
  * 예시

  ![1563245567309](/assets/img/Grid/img25.png)









* **grid-column / grid-row**

  * grid-column-start + grid-column-end 와 grid-row-start + grid-row-end 를 설정하는 방법
  * 예시

  ![1563245678246](/assets/img/Grid/img26.png)







* **grid-area**

  * grid-row-start + grid-column-start + grid-row-end + grid-column-end 를 한 번에 설정하는 방법
  * 예시

  ![1563245862084](/assets/img/Grid/img27.png)







* **justify-self**

  * cell 내부에 존재하는 하나의 grid 아이템을 가로축을 따라 정렬하는 방법

  ![1563245968009](/assets/img/Grid/img28.png)

  * 예시

  ![1563246005276](/assets/img/Grid/img29.png)





* **align-self**

  * justify-self 의 세로 방식

  ![1563246199665](/assets/img/Grid/img30.png)

  * 예제

  ![1563246227611](/assets/img/Grid/img31.png)





* **place-self**

  * justify-self 와 align-self 를 한 번에 설정하는 방법
  * 예시

  ![1563246280994](/assets/img/Grid/img32.png)







## 참고 사이트

* <https://css-tricks.com/snippets/css/complete-guide-grid/>

