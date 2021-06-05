---
layout: post
title: "카드 짝맞추기(feat.이벤트루프)"
subtitle: "자바스크립트 실습 9일차."
date: 2021.06.04 18:00:00
background: "/img/portfolio.png"
---

## **카드 짝맞추기(feat.이벤트루프)**

### `2021.05.12`

#### <span style="color:navy">소스코드: [concentration.html](https://github.com/WaterMinCho/JS/blob/main/concentration.html){: target="\_blank"}</span>

---

### **플로우**

- <center><img src="https://user-images.githubusercontent.com/74204327/117998950-56002280-b37f-11eb-8b41-cc893439d320.png" width="70%" height="auto" alt ="알고리즘"></center>

---

### **배운 개념**

<span style="color:red">1. [concat()](#concat)</span><br>
<span style="color:red">2. [버그](#버그)</span><br>
&emsp;- [호출 스택, 이벤트 루프, 백그라운드, 태스크 큐](#호출-스택-이벤트-루프-백그라운드-태스크-큐)<br>
&emsp;- [버그 해결](#결국-뭐가-문젠데)<br>
<span style="color:red">3. [느낀 점](#느낀-점) </span><br>

---

- #### concat()

  - concat()메서드는 인자로 주어진 배열이나 값들을 기존 배열에 합쳐서 새 배열을 반환한다.
    - 메서드를 호출한 배열 뒤에 각 인수를 순서대로 붙인다.
    - 인수가 배열이면 그 요소가 순서대로 붙고, 배열이 아니면 인수 자체가 붙는다. 중첩 배열 내부로 재귀하지 않는다.
    - `기존 배열을 변경하지 않는다!`
    - 추가된 새 배열을 반환한다.

---

- ### 버그

  - 버그1: 처음 카드 보여줄 때 카드 클릭이 가능하면 안된다.
  - `clickable` 변수로 막을 수 있다.
    ```javascript
    let clickable = false;
    .
    .
    .
    function onCLickCard() {
        if (!clickable) { //clickable이 false면 onClickCard실행 불가.(return으로 아랫줄 실행 막음)
          return;
        }
        .
        .
        .
    }
    function startGame() {
        clickable = false;
        .
        .
        .
        setTimeout(() => {
          document.querySelectorAll(".card").forEach((card) => {
            card.classList.remove("flipped");
          });
          clickable = true; //카드를 감출 때 true값으로 변환.
        }, 5000);
      }
    ```
  - 버그2, 버그3: 이미 짝이 맞춰진 카드를 선택해도 카드가 다시 뒤집힌다. / 한 카드를 두 번 연달아 클릭하면 더 이상 그 카드가 클릭되지 않는다.

  - 버그1에서 수정한 if조건문에 추가한다.

  ```javascript
      function onCLickCard() {
        if (!clickable || completed.includes(this) ||clicked[0]===this) { //이미 완성된 카드거나 연달아 클릭하는 경우
        return;
        }
        .
        .
        .
        }
  ```

  - <u>`버그4`: 서로 다른 네 가지 색의 카드를 연달아 클릭하면 마지막 두 카드가 앞면을 보인 채 남아 있다.(중요. `이벤트루프와 호출스택`을 알아야 수정이 가능하다.</u>
  - #### **호출 스택, 이벤트 루프, 백그라운드, 태스크 큐**
    - **백그라운드**는 타이머를 처리하고 이벤트 리스너를 저장하는 공간이다. `setTimeout`함수가 실행되면 백그라운드에서 시간을 재고 해당 시간이 되면 `setTimeout`의 `콜백 함수`를 `태스크 큐`로 전달한다. <br>
      또한 `addEventListener`로 추가한 이벤트를 저장했다가 이벤트가 발생하면 `콜백 함수`를 `태스크 큐`로 보낸다. `백그라운드`에서 코드를 실행하는 것이 아니라 실행될 `콜백 함수`들이 `태스크 큐`로 들어간다는 것을 명심하자. <br><br>
    - **태스크 큐**는 실행돼야 할 콜백 함수들이 순서대로 대기하고 있는 공간이다. 즉 `태스크 큐`에 먼저 들어온 함수부터 실행된다는 것이다. 하지만 `태스크 큐`도 함수를 직접 실행하지 않는다. <br><br>
    - **이벤트 루프**는 `태스크 큐`에서 `호출 스택`으로 함수를 이동시키는 존재다. `호출 스택`이 비어 있으면 `이벤트 루프`는 `태스크 큐`에서 함수를 순서대로 꺼내어 `호출 스택`으로 옮긴다. 호출 스택으로 이동한 함수는 그때 실행된다. 실행이 완료된 함수는 `호출 스택`에서 빠지고 `호출 스택`이 비어있으면 `이벤트 루프`는 `태스크 큐`에 있는 다음 함수를 `호출 스택`으로 옮긴다.
      <br><br> - 변수나 함수의 선언은 `호출 스택`과 `이벤트 루프`에 영향을 주지 않는다! - 선언은 `스코프`의 영역이다. - `호출 스택`과 `이벤트 루프`는 함수 호출과 밀접한 관련이 있다. - 자바스크립트 엔진은 자바스크립트 소스 코드가 처음 실행되는 순간(script태그)도 하나의 함수가 실행된다고 판단하고 크롬은 이를 `anonymous함수`로 표시한다. 참고로 `호출 스택`을 보고 싶다면 `console.trace`를 사용하여 콘솔창에 출력가능하다.
      <br><br>
  - 실습코드를 보면 호출 스택의 흐름은 anonymous <in> -> startGame() <in> -> shuffle() <in>-> shuffle() <out> -> createCard() <in> -> createCard() <out> -> appendChild() <in> -> appendChild() <out> ... 식으로 흘러가며 `addEventListener`와 `setTimeout()`를 만나면 해당 이벤트는 `백그라운드`에 저장한다.
  - 그럼 `startGame()`이 끝나면 호출 스택에서 대기중인 함수는 사실상 없지만 `백그라운드`에 `클릭이벤트와 타이머`가 남아있다는 사실! 그러면 대부분의 비동기 코드들이 `백그라운드`에 있다고 생각하면 될 것 같다.

    ```javascript
    document.querySelectorAll(".card").forEach((card, index) => {
      setTimeout(() => {
        card.classList.add("flipped");
      }, 1000 + 200 * index);
    });
    ```

  - 위 코드에서 1초타이머의 콜백함수는 `card.classList.add("flipped");` 인데 이 구간이 바로 `태스크 큐`에 쌓여있고 방금같은 상황에 호출스택이 비어있으니 `이벤트루프`가 `태스크 큐`의 해당 함수를 `호출 스택`으로 전달한다(실행을 하려면 `호출 스택`으로 쌓여져있어야 하기 때문).

  - ### 결국 뭐가 문젠데?

    - 4개를 연속 선택했을 때 연속된 카드를 1,2,3,4번 카드라고 가정해보자.<br>

      1. 호출 스택에 클릭 콜백함수를 거쳐 `push()`로 실행되어 clicked === [1번 카드] 상태
      2. 2번 카드의 클릭 콜백 함수가 실행될 때는 clicked배열에 두 카드가 추가
      3. 색이 다르니 `setTimeout`이 실행될거고 0.6초 타이머가 백그라운드에 생성되어 대기상태. 여기서 핵심을 알 수 있다. 태스크 큐에 들어온 순서대로 호출 스택으로 가다보니 0.6초 타이머의 콜백함수보다 3번 카드의 클릭 콜백 함수가 먼저 실행되는 것이다.
      4. 3번의 시점에선 호출 스택으로 `clicked배열`에 [1,2]가 들어가 있고 `태스크 큐`엔 3번4번이 대기중이며, `백그라운드`엔 addEventListener(12), setTimeout 600ms가 대기중이다.
      5. 3번이 호출 스택에 추가되고 1번을 거친다. `clicked`===[1,2,3] / `태스크 큐:` 4번 / `백그라운드:` addEventListener(12), setTimeout 600ms, setTimeout 600ms
      6. `clicked`===[1,2,3,4] / `태스크 큐:` empty / `백그라운드:` addEventListener(12), setTimeout 600ms,setTimeout 600ms, setTimeout 600ms
      7. `clicked`===[1,2,3,4] / `태스크 큐:` setTimeout 600ms,setTimeout 600ms, setTimeout 600ms / `백그라운드:` addEventListener(12)
      8. setTimeout600ms의 콜백함수인

      ```javascript
      clicked[0].classList.remove("flipped");
      clicked[1].classList.remove("flipped");
      clicked = [];
      ```

      를 실행하는데 위의 과정으로 <u>3번과 4번은 remove('flipped')가 적용이 안된 채로 남아있고 clicked 배열을 비우는 것이 문제인걸 확인했다..</u>
      <br><br>
      카드를 뒷면으로 뒤집고 clicked를 []로 초기화하기 전에 3,4번 카드의 클릭 이벤트 콜백이 실행되는게 문제기 때문에 실행되더라도 아무 일도 하지 않게 만들면 된다.
      <br>
      그렇다면 **카드가 2장이 될 때 `clickable`을 `flase`로 만들어서 세 번째 카드부터는 클릭해도 아무것도 하지 않고 끝나게 하자**
      <br>

      ```javascript
      function onCLickCard() {
        if (!clickable || completed.includes(this) || clicked[0] === this) {
          return;
        }
        this.classList.toggle("flipped");
        clicked.push(this);
        if (clicked.length !== 2) {
          return;
        }
        const firstBackColor =
          clicked[0].querySelector(".card-back").style.backgroundColor;
        const secondBackColor =
          clicked[1].querySelector(".card-back").style.backgroundColor;
        if (firstBackColor === secondBackColor) {
          completed.push(clicked[0]);
          completed.push(clicked[1]);
          clicked = [];

          if (completed.length !== total) {
            return;
          }
          setTimeout(() => {
            alert("축하합니다.");
            resetGame();
          }, 1000);

          return;
        }
        clickable = false; //2장이면 false
        setTimeout(() => {
          clicked[0].classList.remove("flipped");
          clicked[1].classList.remove("flipped");
          clicked = [];
          clickable = true; //0.6초 뒤에 true로 설정.
        }, 600);
      }
      ```

- #### 느낀 점
  코드 작성할 때 스코프를 고려해야 하고 짜여진 코드를 분석할 땐 이벤트루프개념을 고려해야겠다는 생각이 들었다. 그리고 실행컨텍스트를 따로 정리하는 시간을 가져야겠다는 생각도 들었다. (반드시 하자!)
