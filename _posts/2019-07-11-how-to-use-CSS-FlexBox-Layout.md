---

layout: post

title:  "FlexBox 가이드"

author: "nenemttin"

date: '2019-07-15 11:07'

categories: [css, flexbox, guide]

---

## 작성 이유

* 웹/모바일 프로젝트를 하면서 vuetify에 의존하지 않고 레이아웃 및 그리드 시스템 사용해보자
* css와 친숙해져보자



## Background

* Flexbox Layout Module은 하나의 컨테이너 안에 크기를 모르거나 동적으로 변하는 아이템들을 효율적으로  레이아웃, 정렬, 공간 분배하기 위한 방법을 제공하는데 초점을 두고 있다. 
* Main idea는 컨테이너에게 그 안에 속한 아이템들의 너비나 높이를 이용 가능한 공간에 가장 알맞게 채우기 위해 그것들을 바꿀 수 있는 능력을 주자는 것이다.
* Note: Flexbox layout은 한 앱의 컴포넌트, 작은 스케일의 레이아웃으로 가장 적절하고 Grid layout 은 조금 더 큰 스케일의 레이아웃에 적합하다.



## Basic & Terminology

* flexbox 는 하나의 전체 모듈이기 때문에, properties의 set을 포함한 많은 것들을 포함하고 있다. 그 중 일부는 컨테이너(parent element, flex container)고 나머지는 children('flex items')이다.



![img1](/assets/img/flexbox/img1.png)

* 아이템들은 다음의 main axis (main-start부터 main-end로) 또는 cross axis(cross-start부터 cross-end로)에 놓여진다.
  * **main axis** - flex container에서 아이템이 놓이는 가장 주요한 축이다. 수평일 필요는 없고, flex-direction property에 따라 달라진다.
  * **main-start | main-end** - 아이템이 start로부터 end로 놓인다
  * **main size** - main dimension에서 flex 아이템들의 너비 혹은 높이 
  * **cross axis** - main axis의 수직 버전, 이것의 방향은 main axis와 같다.
  * **cross-start | cross-end** - 아이템이 start로부터 end로 놓인다
  * **cross-size** - cross dimension에서 flex 아이템들의 너비나 높이

---

![img2](/assets/img/flexbox/img2.png)

### Properties for the Parent(flex container)

* **display** 

  ![img3](/assets/img/flexbox/img3.png)

  * container 속성 설정 (flex, inline, block, inlineblock)****
  * flex - flex box로 만든다.
  * inline - <span>, <b>, <i> 태그 등이 해당, block과 달리 줄 바꿈이 되지 않고, width, height를 지정할 수 없음
  * block - <div> / <p> 등이 해당, 가로 길이가 기본적으로 100%, block인 태그를 이어서 사용하면 줄바꿈 되어 보임, width, height 속성 지정할 수 있으며, 레이아웃 배치시 주로 쓰임
  * inlineblock - block과 inline의 중간 형태라고 볼 수 있는데, 줄 바꿈이 되지 않지만 크기를 지정할 수 있습니다. internet Explorer 7 이하 사용 불가.
  * 참고 : [https://ofcourse.kr/css-course/display-%EC%86%8D%EC%84%B1](https://ofcourse.kr/css-course/display-속성)

  



* **flex-direction**

  ![img4](/assets/img/flexbox/img4.png)

  * row(default) : 왼 -> 오
  * row-reverse: 오-> 왼
  * column: 위 -> 아래
  * column-reverse: 아래 -> 위



* **flex-wrap**

  ![img5](/assets/img/flexbox/\img5.png)

  * nowrap(default): 모든 아이템들을 한 줄에 둔다
  * wrap: 아이템들을 여러 줄에 둔다. 순서는 위 -> 아래
  * wrap-reverse: 아이템들을 여러 줄에 둔다. 순서는 아래-> 위



* **flex-flow**

  ![img6](/assets/img/flexbox/img6.png)

  * flex-direction 과 flex-wrap properties의 축약어로 컨테이너의 main과 cross 축을 함께 정의, 기본 값은 row nowrap



* **justify-content**

  ![img7](/assets/img/flexbox/img7.png)

  * main axis를 따라 정렬한다. 아래 그림으로 확인

  

  ![img8](/assets/img/flexbox/img8.png)

  

* **align-items**

  ![img9](/assets/img/flexbox/img9.png)

  * cross axis를 따라 아이템들을 정렬한다.

  ![img10](/assets/img/flexbox/img10.png)



* **align-content**

  ![img11](/assets/img/flexbox/\img11.png)

  * cross axis를 따라 컨테이너의 라인들을 정렬한다.
  * Note: 이 속성은 아이템들의 라인이 하나면 효과가 없다.

  ![img12](/assets/img/flexbox/\img12.png)

---

### Properties for the Children(flex items)

![img13](/assets/img/flexbox/img13.png)



* **order**

  ![img14](/assets/img/flexbox/img14.png)

  * flex container 안에서 보여지는 순서를 제어한다.



* **flex-grow**

  ![img15](/assets/img/flexbox/img15.png)

  * 필요할 때 flex 아이템이 커지는 것
  * container 안에서 모든 아이템의 flex-grow 값이 1이라면, 공간이 균등하게 분배되어 나타나고, 한 아이템의 값이 2라면 1값을 가지는 아이템의 2배의 길이로 나타난다.
  * 음수 값은 유효하지 않음



* **flex-shrink**

  ![img16](/assets/img/flexbox/img16.png)

  * grow 의 반대 개념
  * 음수 유효하지 않음



* **flex-basis**

  ![img17](/assets/img/flexbox/img17.png)

  * flex 아이템의 크기를 결정, 숫자나 키워드로 정의 가능
  * flex 아이템에 크기가 지정되어 있지 않으면 내용물 크기가 flex-basis 값으로 사용됨
  * flex container에 display :flex 속성만을 지정하면 flex 아이템들이 각 내용물 크기만큼 공간을 차지하게 됨



* **flex**

  ![img18](/assets/img/flexbox/img18.png)

  * flex 속성들 한꺼번에 지정하고 싶을 때 사용



* **align-self**

  ![img19](/assets/img/flexbox/img19.png)

  * 개별 flex 아이템들의 정렬을 가능하게 함
  * Note: float, clear, vertical-align은 flex 아이템에 영향을 주지 않는다.



## 참고 사이트

* <https://css-tricks.com/snippets/css/a-guide-to-flexbox/>

