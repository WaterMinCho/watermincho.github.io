---
layout: post
title: "지뢰찾기"
subtitle: "자바스크립트 프로젝트 1회차 <br>
배운 내용으로 프로젝트 만들기(1/3)"
date: 2021.06.05 14:00:00
background: "/img/portfolio.png"
---

## **지뢰찾기(자바스크립트 프로젝트 1회차)**

### `2021.05.14`

#### <span style="color:navy">소스코드: [mine-sweeper.html](https://github.com/WaterMinCho/JS/blob/main/mine-sweeper.html){: target="\_blank"}</span>

---

### **플로우**

- 기본적으로 좌클릭을 했을 때랑 우클릭을 했을 때를 나눠서 처리해야 한다.

  - <center><img src="https://user-images.githubusercontent.com/74204327/118597672-f2934c00-b7e7-11eb-8db1-3b9a8595c87e.png" width="80%" height="auto" alt ="알고리즘"></center>

  - <center><img src="https://user-images.githubusercontent.com/74204327/118239686-11869b00-b4d5-11eb-96ef-66e8e38a1473.png" width="80%" height="auto" alt ="알고리즘"></center>

---

### **배운 개념**

<span style="color:red">1. [상태에 따른 코드 정의](#상태에-따른-코드-정의)</span><br>
<span style="color:red">2. [코드 작성 순서](#순서)</span><br>
<span style="color:red">3. [Optional Chaining](#optional-chaining)</span><br>
<span style="color:red">4. [nullish coalescing operator](#nullish-coalescing-operatornull-병합-연산자) </span><br>
<span style="color:red">5. [Maximum call stack size exceeded 에러](#maximum-call-stack-size-exceeded-에러) </span><br>

---

- ### 상태에 따른 코드 정의

  상태값에 따른 코드를 정의할 때 보통 숫자로 지정하는데 여러 번 시뮬레이션을 한 뒤에 최적의 숫자조합을 찾는 것이 현명하다.<br>
  코드숫자는 0~8까지 열린 칸<br>
  지뢰가 없을 때 `닫힌 칸(NORMAL)`은 -1, `물음표칸(QUESTION)`은 -2, `깃발 칸(FLAG)`은 -3.<br>
  지뢰가 있을 때 `닫힌 칸은(MINE)` -4, `물음표칸(QUESTION_MINE)`은 -5, `깃발 칸(FLAG_MINE)`은 -6
  <br>

---

- ### 순서

  1. 배열`candidate`에 빈 공간을 만들고 그 공간에 0부터 99까지 채워넣는다.
  2. `chosen`배열에 `mine`갯수인 10개를 뺀 공간에 랜덤으로 채워넣는다.
  3. 이차원 배열로 `rowData`배열에 `CODE.NORMAL(-1)`을 채워넣는다.
  4. 랜덤하게 생성된 지뢰 칸 위치(shuffle[k])에 MINE(-6)을 data[][]에채워넣는다.
  5. `onRightClick`함수 생성 후 `preventDefault`로 기본동작 제거. `target`으로 html코드 불러올 수 있음.
  6. `$tbody.addEventListener("contextmenu", onRightClick);`에서 `contextmenu`는 우클릭이벤트인데 해당 이벤트 콜백을 `onRightClick`으로 적용.
  7. `onLeftClick()`을 선언 후 닫힌 칸인지 지뢰칸(`지뢰칸이면 removeEventListener.`)인지 여부 판단 함.
  8. `countMine()`도 정의해서 주변 지뢰갯수를 카운트한다.
  9. 지뢰게임은 지뢰를 제외한 나머지 칸을 모두 열면 승리하는 게임이고 주변 지뢰개수가 0개인 칸을 클릭하면 자동으로 주변 칸을 모두 열어주는 기능도 넣어야 한다.
  10. `openAround(), isNormal(), open()`을 정의해서 `open이 0`이면 `openAround`가 주변을 열어준다.
  11. 승리하는 케이스일때 추가. 100칸 중에 10칸을 제외한 90칸을 모두 열면 승리하는 조건 추가하면 된다.
  12. 줄, 칸, 지뢰 개수 조절하기-->row,cell,mine변수에 input으로 submit할 수 있도록 세팅. submit하면 openCount초기화, tbody지우고 drawTable()실행, timer시작. --> 해당하는 변수들을 let으로 수정.

---

- ### Optional Chaining

  - `?.`
    - 앞 평가 대상이 undefined나 null이면 평가를 멈추고 undefined를 반환한다. `.?`는 존재하지 않아도 괜찮은 대상에만 사용해야 한다.<br>
      자바스크립트에선 배열에 음수번째 인덱스를 넣으면 `undefined`가 난다.`(undefined[-1])`
    ```javascript
    if (data[-1]) {
      data[-1][-1];
    }
    ```
    이런 식으로 커버할 수 있으며, 또는 접근자 `. 앞에 ?`를 붙이면 조건접근이 가능하다.
  - `mines.includes(data[rowIndex - 1]?.[cellIndex - 1]) && i++;`
    - data[][]가 존재하면 i++실행.
  - 문법
    ```javascript
    obj?.prop;
    obj?.[expr];
    arr?.[index];
    func?.(args);
    ```
  - .연산자는 `nullish참조값`일 때 에러를 뱉고 `?.`연산자는 에러 대신 `undefined`를 리턴하는 차이가 있다.
  - ```javascript
    let aProp = obj.first?.second;
    ```
    `obj.first.second`에 접근하기 전에 `obj.first가 nullish가 아니라는 것을 암묵적으로 확인했다는 뜻`이다. 만약 `obj.first가 nullish라면` 그 표현식은 자동으로 잘리고 `undefined가 반환`된다. 추가로 아래와 같이 풀어낼 수 있다. (참고:MDN)
    ```javascript
    let aProp =
      obj.first === null || obj.first === undefined
        ? undefined
        : obj.first.second;
    ```

---

- ### nullish coalescing operator(null 병합 연산자)

  `??, ||, &&`를 추가로 설명하면 기존에 and, or처럼 판단을 해왔지만 엄밀히 논리적으로 따지면 아래와 같다.

  - target.textContent = A || B; <br>
    `A가 존재하지 않으면(false면) B.` `존재하면(true면) A.`
  - target.textContent = A ?? B; <br>
    `A가 Null 또는 Undefined면 B.` `그 외엔 A.`
  - mines.includes(A&&B);<br>
    `A가 존재하면(true면) B.` `존재하지 않으면(false면) A.`

---

- ### Maximum call stack size exceeded 에러
  - 재귀할 때 자주 나오는 오류 중 하나.
  - 최대 호출 스택 사이즈를 초과했다는 의미. `호출 스택`의 관점으로 봐야한다.
    - `OnLeftClick()`위에 `openAround()`가 여러 개가 얹어지면서 호출만 하고 끝나지 않는 상황이 발생하여 `호출 스택`의 사이즈를 넘어가는 상황이 벌어진다.
    - 그럴 땐 비동기 코드로 감싸주고(`setTimeout()`) `백그라운드`와 `태스크 큐`에 할당하면 된다.
