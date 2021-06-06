---
layout: post
title: "두더지 잡기 게임"
subtitle: "자바스크립트 프로젝트 3회차 <br>
배운 내용으로 프로젝트 만들기(3/3)"
date: 2021.06.06 18:00:00
background: "/img/portfolio.png"
---

## **두더지 잡기 게임 (자바스크립트 프로젝트 3회차)**

### `2021.05.21`

#### <span style="color:navy">소스코드: [wrack-a-mole.html](https://github.com/WaterMinCho/JS/blob/main/wrack-a-mole.html){: target="\_blank"}</span>

### **플로우**

---

- <center><img src="https://user-images.githubusercontent.com/74204327/119090679-e2c57300-ba46-11eb-8f35-1611840a1f71.png" width="110%" height="auto" alt ="2048"></center>

- <center><img src="https://user-images.githubusercontent.com/74204327/119090995-55cee980-ba47-11eb-981c-cd38ec1665a0.png" width="65%" height="auto" alt ="2048"></center>

---

### **배운 개념**

<span style="color:red">1. [순서](#순서)</span><br>
<span style="color:red">2. [HTML과 CSS활용](#html과-css활용)</span><br>
<span style="color:red">3. [이벤트 루프](#이벤트-루프)</span><br>
<span style="color:red">4. [alert함수 사용 시 주의점](#alert함수-사용-시-주의점)</span><br>

- #### 순서

  1. 두더지와 폭탄 선택하기.
  2. setInterval과 setTimeout으로 나오고 들어가는 간격 설정하기.
  3. if문으로 폭탄이랑 두더지 비율 설정하기.
  4. 마우스 이벤트 받기.
  5. 전체 타임아웃 설정하고 스코어 저장하면서 게임 종료.
  6. 같은 두더지 두 번 눌렀을 때 방지.
  7. life를 지정하여 폭탄

---

- #### HTML과 CSS활용

  - 애니메이션이나 화면 요소들의 배치에 관한 것은 자바스크립트보다는 HTML과 CSS에 맡기는 것이 더 좋다.

---

- #### 이벤트 루프

  - 이벤트가 많이 발생하는 경우에 프로그램 전체에서 발생하는 이벤트를 모두 분석하면 복잡하다. 이럴 떄는 관련있는 이벤트끼리만 분석해도 된다. <u>어떠한 이벤트를 분석하는데 영향을 미치지 않는 다른 이벤트가 있다면 해당 이벤트는 이벤트 루프 분석에서 제외할 수 있다.</u>

---

- #### alert함수 사용 시 주의점

  - `alert` 함수는 현재 진행되는 화면 변경 사항이나 애니메이션을 즉시 멈추고 알림 창을 띄우므로 창이 뜰 때 마지막 화면 변경 사항이나 애니메이션이 적용되지 않는 경우가 많다. 이럴 땐 `setTimeout`과 함께 호출해서 마지막 화면 변경 사항이나 애니메이션이 적용될 시간을 주는 것이 좋다.

13파트동안 공부하면서 해당 자료와 강의를 제공해주신 [제로초(ZeroCho)](https://www.zerocho.com/){: target="\_blank"}님에게 감사하다는 말씀 전하고 싶다. React.js, node.js, typescript 마스터할 때까지 열심히 공부하겠습니다!
