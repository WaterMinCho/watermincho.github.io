---
layout: post
title: "숫자야구"
subtitle: "자바스크립트 실습 3일차.<br>"
date: 2021.05.30 01:00:00
background: "/img/portfolio.png"
---

## **숫자야구**

### `2021.04.29`

#### <span style="color:navy">소스코드: [number-baseball.html](https://github.com/WaterMinCho/JS/blob/main/number-baseball.html){: target="\_blank"}</span>

---

### **플로우**

- 최초 플로우
  <center><img src="https://user-images.githubusercontent.com/74204327/116469803-922a8200-a8ad-11eb-80d5-03abfac0124a.png" width="75%" height="auto"></center>
  <!-- <p align = "center"> <img src = https://user-images.githubusercontent.com/74204327/116469803-922a8200-a8ad-11eb-80d5-03abfac0124a.png width=500px></p> -->

- 두 번째 플로우(3진 아웃 추가)
<center><img src="https://user-images.githubusercontent.com/74204327/116470169-02390800-a8ae-11eb-85be-4fb506e2cde9.png" width="65%" height="auto"></center>
  <!-- <p align = "center"> <img src =https://user-images.githubusercontent.com/74204327/116470169-02390800-a8ae-11eb-85be-4fb506e2cde9.png width=500px></p> -->

---

### **배운 개념**

- event.preventDefault(); //기본 동작 막기(메소드이며 form태그의 기본동작인 깜빡거림을 막을 수 있다.)

---

- `form태그`의 `event.target`은 배열 식으로 내부 요소들에 접근이 가능하다.
  예를 들면 `event.target[0], event.target[1]`식으로 각각의 요소를 SELECT할 수 있다.

---

- ```javascript
  for (let n = 0; n < 4; n++) {
    const index = Math.floor(Math.random() * (numbers.length - n));
    answer.push(numbers[index]);
    numbers.splice(index, 1);
  }
  ```

  - 무작위로 숫자를 뽑을 때는 `Math.random`메서드를 사용한다. 단, 뽑은 값은 정수가 아니므로 정수가 필요할 때는 `Math.floor`나 `Math.ceil`같은 메서드를 사용해 정수로 바꿔야 한다.

---

- **indexOf와 includes**

  ```javascript
  "2345".indexOf(3) === 1;
  "2345".indexOf(6) === -1;
  ["2", "3", "4", "5"].indexOf("5") === 3;
  ["2", "3", "4", "5"].indexOf(5) === -1; // 요소의 자료형까지 같아야 한다.
  "2345".includes(3) === true;
  ["2", "3", "4", "5"].includes(5) === false;
  ```

  - 공통점
    - 배열이나 문자열에 원하는 값이 들어있는지 찾는 메서드다.
  - `indexOf`: 원하는 값이 들어있다면 해당 인덱스까지 알려주고, **들어있지 않다면 -1반환**
  - `includes`: 더 직관적으로 true/false를 반환한다.

---

- ```javascript
  if (new Set(input).size !== 4) {
    return alert("중복되지 않게 입력해주세요");
  }
  ```

  - **new Set(input).size !== 4**
    - **Set은 중복을 허용하지 않는 특수한 배열**이다.
    - `new Set('1231')`을 하면 `Set` 내부에는 1,2,3만 들어간다. 그러므로 위의 코드에 중복이 없다면 4가 나오지만 중복이 있다면 4보다 작은 값이 나올 것이다.
    - Set의 길이를 구할 때는 `.length`가 아닌 `.size`로 배열의 길이를 잰다.
  - **undefined를 return한다.** 즉 `return undefined`와 같고, `undefined`는 if문에서 `false`로 처리하므로 `checkInput()`함수에 `false`로 반환한다.

---

- ```javascript
  if (answer.join("") === value) {
    $logs.textContent = "홈런!";
    return;
  }
  ```

  - `.join()`
    - 배열을 문자열로 바꾸는 메소드.
    - ('')하면 [3,1,4,5]라는 배열이 '3145'라는 문자열로 바뀜. 안하면 '3,1,4,5'로 불러옴.<기본 값 = (',')>

---

- ```javascript
  if (tries.length >= 9) {
    const message = document.createTextNode(
      "패배. 정답은 ${answer.join("")}"
      );
      $logs.appendChild(message);
      return;
      }
  ```

  - `tries.length>=`를 한 이유: `tries=[]`에 9개가 차 있다면, 10번째 시도 내에서 성공하면 홈런이고 9개가 이미 차있는 상태에서 10번째 시도했을 때 결국 성공하지 못했다면 패배로 출력하도록 하기 위함이다.
  - `.append()`
    - `append`메서드는 새로 만들고 싶은 값을 바로 옆에 추가한다.
    - `createElement`를 통해 각각의 `텍스트`와 `html`요소를 지정하여 추가가 가능하다.
    - `appendChild()`는 `createTextNode`를 사용하고 변수에 저장 후에 변수를 인자로 넣어야한다. --> `append`는 `appendChild`를 보완해서 만든거다.<br>
