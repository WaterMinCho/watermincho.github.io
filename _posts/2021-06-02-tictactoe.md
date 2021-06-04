---
layout: post
title: "틱택토(+ 컴퓨터전)"
subtitle: "자바스크립트 실습 7일차.<br>"
date: 2021.06.02 15:00:00
background: "/img/portfolio.png"
---

## **틱택토**

### `2021.05.03`

#### <span style="color:navy">소스코드: [tictactoe.html](https://github.com/WaterMinCho/JS/blob/main/tictactoe.html){: target="\_blank"}</span>

---

### **플로우**

- 3x3 표 위에서 삼목과 같은 룰로 진행하는 게임이다. 자바스크립트에선 이차원 배열로 표현한다.

- <center><img src="https://user-images.githubusercontent.com/74204327/116865069-e635bd80-ac43-11eb-846f-d34b6f1dfc49.png" width="80%" height="auto"></center>
    <!-- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116865069-e635bd80-ac43-11eb-846f-d34b6f1dfc49.png height = 350px width = 430px></p> -->

---

### **배운 개념**

<span style="color:red">1. [구조분해 할당](#구조분해-할당)</span><br>
<span style="color:red">2. [이벤트 버블링 / 이벤트 캡처링](#이벤트-버블링)</span><br>
&emsp;- [유사배열 객체](#유사배열-객체)<br>
<span style="color:red">3. [틱택토 컴퓨터전](#틱택토-컴퓨터전) </span><br>

---

- #### **구조분해 할당**

  `배열`이나 `객체`의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 JavaScript 표현식.
  속성명과 변수명이 일치하면 그 갯수가 어떻든 여러 개 표현 가능하다.

  ```javascript
  //객체 구조 분해
  const { body, createElement } = document;
  // 아래와 같다.
  // const body = document.body
  // const createElement = document.createElement

  const o = { p: 30, q: true };
  const { p, q } = o;

  //선언 없는 할당
  const p, q;
  ({ p, q } = { p: 30, q: true });

  //배열 구조 분해
  const arr = [1, 2, 3, 4, 5];
  const [one, two, three, four, five] = arr;
  // 아래와 같다.
  /*
  const arr = [1,2,3,4,5];
  const one = arr[0];
  const two = arr[1];
  const three = arr[2];
  const four = arr[3];
  const five = arr[4];
  */
  ```

  위와 같이 객체와 배열에 접근하여 할당이 가능하다. `함수도 그렇다.`<br>
  아래 예제에서 f()는 출력으로 배열[1, 2]을 반환하는데, 하나의 구조 분해만으로 값을 분석할 수 있다.

  ```javascript
  function f() {
    return [1, 2];
  }

  const a, b;
  [a, b] = f();
  console.log(a); // 1
  console.log(b); // 2
  ```

  변수에 배열의 나머지 할당할 때, 나머지 구문을 이용해 분해하고 `남은 부분`을 하나의 변수에 할당할 수 있다.

  ```javascript
  const [a, ...b] = [1, 2, 3];
  console.log(a); // 1
  console.log(b); // [2, 3]
  ```

  그럼 아래의 코드에서 a,c,e속성과 a,b,e을 구조 분해 할당 문법으로 변수에 할당해보자.

  ```javascript
  const obj = {
    a: "hello",
    b: {
      c: "hi",
      d: { e: "wow" },
    },
  };

  //a,c,e
  const {
    a,
    b: {
      c,
      d: { e },
    },
  } = obj;

  //a,b,e
  const { a, b } = obj;
  const {
    d: { e },
  } = b;
  ```

  `리액트`에서도 객체 구조 분해 할당을 예로 들 수 있다.

  ```javascript
  //...
  this.state = {
    like: 0,
    follow: 0,
  };
  //...

  const { like, follow } = this.state;
  ```

  `props`를 받아오거나 `클래스형 컴포넌트`에서의 `state` 값을 쓸 때 구조 분해 할당을 이용해주면 반복되는 당연한 단어들을 줄여줘서 좀 더 편리하다<br>
  마지막으로 로그인 로직의 배열구조 분해할당을 예로 들겠다.

  ```javascript
  //...
  const [shouldBeBearer, token] = req.headers.authorization.split(" ");
  //...
  ```

  로그인 시 발급되는 token을 xxxx xXxxXXXxXxxxxX-XxX...식으로 string형태로 넘어오는데, 띄어쓰기 단위로 split 뒤에 Bearer(xxxx)과 token을 분리하면 앞부분은 필요가 없으니까 token값만 가져다 쓸 수 있다.

  뭔가... 어제 마지막으로 필수적인 내용들을 정리했었는데, 구조분해 할당까지도 마스터한다면 두려울 것이 없어보인다. ~~이불킥~~<br><br>

---

- #### **이벤트 버블링**

  - 실습과 같이 `td태그`에 `addEventListener`를 적용하였는데 실제로 `html`은 `td`의 부모 `tr->table~body태그`까지 모두 클릭이벤트를 받고 있다. 해당 현상을 <u>이벤트 버블링</u>이라고 한다.<br>
    위와 같이 중첩된 요소에서 이벤트가 발생할 때, `HTML DOM API`의 이벤트 전파방식은 `두 가지`가 존재하는데 바로 <u>버블링</u>과 <u>캡처링</u>이다.
    - 버블링: 이벤트가 발생한 요소`(event.target)`부터 window까지 이벤트를 전파한다.
    - 캡처링: window로부터 이벤트가 발생한 요소까지 이벤트를 전파한다.
    - 차이점: 전파 방향의 차이.
  - `WC3`에서 명시한 이벤트 플로우다.
    <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116874900-06ba4380-ac55-11eb-9388-4bde5fe32f79.png height = 420px width = 390px></p>
  - `그럼 어떻게 컨트롤하는가?`

    - `캡처링`과 `버블링`은 이벤트를 등록할 때, 정의할 수 있다.<br>
      이벤트 리스너인 `addEventListener` 메소드를 보겠다.<br><br>
      **<code>target.addEventListener(type, listener[, useCapture]);</code>**<br><br>
    - 세번째 인자인 `useCapture` 는 용어 그대로 `캡처링 여부`를 뜻한다.<br>
      `default` 값이 `false` 이기 때문에, 단순히 사용했다면 버블링을 통해 이벤트를 전파했을 것이다.<br>
      `true` 로 설정해주면 캡처링을 통해 이벤트를 전파한다.<br><br>
      **<code>target.addEventListener("click", function(){}, true);</code>**<br><br>
    - <u>이벤트 전파를 원하지 않는다면</u> 단순히 `e.stopPropagation() `메소드를 사용하면 된다.<br>
      ```javascript
      target.addEventListener("click", function (e) {
        e.stopPropagation();
      });
      ```
    - 실습에서는 `event.target`이 최하위 요소인 td를 가리키는데, 만약 table을 가리키고 싶으면 `event.currentTarget`(실제 이벤트가 발생한 요소)을 사용하면 된다. 실무에선 currentTarget만 쓰는 사람들도 많다고 하는데, 굳이 제한을 두기 보다는 버블링, 캡처링 현상을 잘 이해하여 적용하면 된다.<br>
      참고: 이벤트캡처링을 써서 모달 또는 팝업외 영역 선택 시 창이 닫히는 기능을 구현할 수 있다고 한다. 나중에 해봐야겠다.

    - ```javascript
      let rowIndex = 0;
      let cellIndex = 0;
      rows.forEach((rpw, ri) => {
        rows.forEach((cell, ci) => {
          if (cell === target) {
            rowIndex = ri;
            cellIndex = ci;
          }
        });
      });
      ```
      실습 중 행과 열의 위치를 저장해주는 반복문의 코드를 아래와 같이 바꿀 수 있다.
      ```javascript
      const rowIndex = target.parentNode.rowIndex; //td의 부모태그인 tr을 가져오기 위해선 parentNode를 사용하면 된다.
      const cellIndex = target.cellIndex;
      rows.forEach((rpw, ri) => {
        rows.forEach((cell, ci) => {
          if (cell === target) {
            rowIndex = ri;
            cellIndex = ci;
          }
        });
      });
      ```
      바로 위 부모태그를 선택하고 싶으면 `.parentNode`를 쓰면 된다. 반대로는 `children`이 있다. `children`이 여러 개가 있으면 `유사배열 객체`로 반환한다.
    - #### **유사배열 객체**
      `태그.children`은 배열처럼 생긴 객체다. `{0:td, 1:td, 2:td, length:3}`과 같은 모양을 가진 객체다. 배열로 착각하기 쉬운데, `indexOf` 등 배열메서드를 쓰려면 `Array.from`메서드로 유사 배열 객체를 진짜 배열로 바꿀 수 있다.
    - ```javascript
      let draw = true;
      rows.forEach((row) => {
        row.forEach((cell) => {
          if (!cell.textContent) {
            //한 칸이라도 비어있으면
            draw = false; // 무승부가 아니다.
          }
        });
      });
      ```
      위의 2중 반복문에 한칸이라도 비어있을 시에 무승부가 아님을 찾아내는 부분이 있는데, 만약 게임이 다 끝났는데도 for문을 도는 경우가 있으면 그건 최악의 코드가 될 수도 있다.<br>
      `forEach`는 `break, return`으로는 멈출 수 없지만 배열에서 제공하는 `every함수`를 쓰면 된다. 더불어 `some`도 살펴볼거다.<br>
      `array.flat()`으로 2차원 배열을 1차원으로 펴준 뒤에 `every`를 쓰면 된다.
      ```javascript
      rows.flat().every((td) => td.textContent); //td.textContent가 모두 존재해야 true. 아니면 false
      rows.flat().every((td) => !td.textContent); //모두가 존재하지 않으면 true. 아니면 false
      rows.flat().some((td) => td.textContent); //하나라도 존재하면 true. 아니면 false
      rows.flat().some((td) => !td.textContent); //하나라도 없으면 true. 아니면 false
      ```
      즉 실습의 무승부 판단하는 2차원 배열을
      ```javascript
      const draw = rows.flat().every((td) => td.textContent);
      ```
      로 바꿀 수 있다.
      - `flat()`은 3차원 배열을 2차원으로, 2차원 배열을 1차원으로, 1차원은 1차원으로 반환한다.

---

## **틱택토 컴퓨터전**

### `2021.05.03`

#### <span style="color:navy">소스코드: [tictactoe-self.html](https://github.com/WaterMinCho/JS/blob/main/tictactoe-self.html){: target="\_blank"}</span>

---

### **플로우**

- <center><img src="https://user-images.githubusercontent.com/74204327/116888857-e85d4380-ac66-11eb-92b0-1893ad4ddef8.png " width="90%" height="auto"></center>
  <!-- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116888857-e85d4380-ac66-11eb-92b0-1893ad4ddef8.png height = 350px width = 430px></p><br> -->

- 이전 틱택토에서 컴퓨터가 한번 더 진행해서 컴퓨터의 턴인지 물어보고 승패여부 따지는 식으로 반복

### **배운 개념**

- `filter(()=>)` : 조건()에 맞는 데이터 필터링
  - 빈칸인 데이터를 찾아서 그 데이터 길이만큼 반복하여 랜덤함수로 랜덤값 추출할 수 있게 한다.

---

- 핵심은 timeOut실행도중 클릭 못하도록 처리하는 것(clickable 플레그 사용)이랑 게임이 끝났는데 클릭이 되도록 막는 것이다.($table.removeEventLisener('click', callback))
