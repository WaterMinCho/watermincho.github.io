---
layout: post
title: "계산기"
subtitle: "자바스크립트 실습 2일차.<br>
자바스크립트 실습 내용을 블로그에 올리며 복습하기." 
date: 2021.05.29 23:55:00
background: "/img/portfolio.png"
---

## **계산기**

### `2021.04.28` [calculator.html](https://github.com/WaterMinCho/JS/blob/main/calculator.html)

<br><br>

### **플로우**

1. 시작 -> 숫자 1을 저장할 변수생성 -> 연산자를 저장할 변수생성 -> 숫자2를 저장할 변수생성 -> 대기
2. 숫자 버튼 클릭 -> 숫자를 변수에 저장한다{operator변수가 비어있는가 ? numOne=숫자 : numTow=숫자} -> 대기
3. 연산자 버튼 클릭 ->numOne값이 존재하는가? 연산자를 변수에 저장한다:alert('연산자를 입력해주세요') -> 대기
4. =버튼 클릭 -> numTwo값이 존재하는가? 숫자1과2를 연산자를 적용하여 계산한다: alert('두번째 수를 입력해주세요')-> 계산 결과를 화면에 출력한다. -> 끝

### **배운 개념**

- 변수를 바꾸면 화면을 바꾸는 것을 까먹지 말자! --> `$result.value += "0";`

- 함수의 (미친듯한)중복을 제거할 때

  - 달라지는 부분만 매개변수로 만들자
  - 고차함수를 써서 줄일 수 있다.(함수 내에 함수 존재)

- `고차함수`<br>
  click(~~~, onClickNumber('n'));의 onClickNumber가 있는 자리는 함수자리이므로 "함수값으로 리턴된 값"이 와야한다!
  추가로 onClickNumber()함수를 불러오면 undefined가 자동으로 도출되기 때문에 return에 함수형으로 정의해야하며, 중복제거시에 유용한 고차함수를 써서 더 줄이자!<br>

  ## 2021.04.30 추가

  - function()는 함수 자체가 아니다. function()은 함수의 리턴값을 가리킨다.
  - function은 함수 자체를 가리킨다.
  - 따라서 return을 따로 지정하지 않으면 undefined가 리턴되기 때문에 function()은 undefined를 가리킨다!

  예시:

  ```javascript
  const fuc = (msg) => {
    return () => {
      console.log(msg);
    };
  };
  ```

  중괄호와 리턴을 생략할 수 있으니 요놈이<br>

  ```javascript
  const fuc = (msg) => () => {
    console.log(msg);
  };
  ```

  이렇게 바뀌는데 이러한 코드를 보면 항상 머리로만 하지 말고 직접 쳐보고 비교해보는 버릇을 기르자. 원리는 이렇다.<br>

  ```javascript
  const fuc =(msg) => "{"
  "return"() =>{
     console.log(msg);
     }
  "}"
  ```

  위와 같이 쿼테이션("")으로 묶은 부분을 제거한거다.<br><br>

- **event**: click시 함수가 호출되면 브라우저가 `onClickNumber()`식으로 함수를 실행하는데, 그때 인자값으로 `event`를 넣어주는데 그때 함수에 `event`가 전달된다.

- \***\*_중첩 반복문 해결하는 법_\*\***

  ```javascript
  function test() {
    let result = "";
    if (a) {
      if (!b) {
        result = "c";
      }
    } else {
      result = "a";
    }
    result += "b";
    return result;
  }
  ```

  <br>

  1. 'if문 다음에 나오는' 공통된 절차를 각 분기점 내부에 넣는다.

  ```javascript
  function test() {
    let result = "";
    if (a) {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    } else {
      result = "a";
      result += "b";
      return result;
    }
  }
  ```

  <br>

  2. 분기점에서 '짧은 절차'부터 실행하게 if문을 작성한다.

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      //기존조건에서 반대 조건으로 변경.
      result = "a";
      result += "b";
      return result;
    } else {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    }
  }
  ```

  <br>

  3. 짧은 절차가 끝나면 `return`이나 `break`로 중단한다.

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      result = "a";
      result += "b";
      return result; //이미 리턴이 있다.
    } else {
      if (!b) {
        result = "c";
      }
      result += "b";
      return result;
    }
  }
  ```

  <br>

  4. `else`를 제거한다.(이때 중첩 하나가 제거된다.)

  ```javascript
  function test() {
    let result = "";
    if (!a) {
      result = "a";
      result += "b";
      return result;
    }
    if (!b) {
      result = "c";
    }
    result += "b";
    return result;
  }
  ```

  <br>

  5. 다음 중첩된 분기점이 나올 때 1~4의 과정을 반복한다.<br><br>

- -, \*, /는 문자열이 저절로 숫자로 바뀌기 때문에 `parseInt`를 안써도 된다.
