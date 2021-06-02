---
layout: post
title: "반응속도 테스트"
subtitle: "자바스크립트 실습 6일차.<br>"
date: 2021.06.02 01:00:00
background: "/img/portfolio.png"
---

## **반응속도 테스트**

### `2021.05.02`

#### <span style="color:navy">소스코드: [response-check.html](https://github.com/WaterMinCho/JS/blob/main/response-check.html){: target="\_blank"}</span>

---

### **플로우**

- <center><img src="https://user-images.githubusercontent.com/74204327/116807565-d39d8480-ab6e-11eb-8c35-577dbf3b6aab.png" width="70%" height="auto"></center>
  <!-- <p align="center"><img src = https://user-images.githubusercontent.com/74204327/116807565-d39d8480-ab6e-11eb-8c35-577dbf3b6aab.png height = 600px width = 700px></p> -->

---

### **배운 개념**

<span style="color:red">1. [Date객체](#date객체)</span><br>
<span style="color:red">2. [let](#let의-스코프)</span><br>
<span style="color:red">3. [const](#const)</span><br>
<span style="color:red">4. [스코프란?](#그래서-스코프가-뭐야) </span><br>
<span style="color:red">5. [Reduce](#reduce) </span><br>
<span style="color:red">6. [일지](#일지-프론트엔드-개발자가-되기로-한-이유와-고찰) </span><br>

---

- `HTML`에서 요소에 대한 정보를 저장할 때가 있으니 최대한 이것을 활용하자.
  - 클래스명을 사용하기: 태그에 해당 클래스가 들어있는지는 `TAG.classList.contains('클래스'); = T/F`로 찾을 수 있다.
    - `TAG.classList.add('클래스')`; -->추가 <br>
      `TAG.classList.replace('클래스')`; -->수정 <br>
      `TAG.classList.remove('클래스')`; -->제거

---

- #### Date객체

  - `new Date(2021, 06, 02, 18, 30, 5)`; --> `javascript`는 `'월'`만 0 index부터 시작한다.<br>
    결과: `Sun May 02 2021 18:30:05 GMT+0900(대한민국 표준시)`

---

- ### **`let의 스코프`**

  ```javascript
  $screen.addEventListener("click", (event) => {
    if (event.target.classList.contains("waiting")) {
      $screen.classList.remove("waiting");
      $screen.classList.add("ready");
      $screen.textContent = "초록색이 되면 클릭하세요";
      setTimeout(() => {
        let startTime = new Date(); // 선언
        $screen.classList.remove("ready");
        $screen.classList.add("now");
        $screen.textContent = "클릭하세요!";
      }, Math.floor(Math.random() * 1000) + 2000);
    } else if (event.target.classList.contains("ready")) {
    } else if (event.target.classList.contains("now")) {
      let endTime = new Date(); // 선언
      result = endTime - startTime;
      $result.textContent = `${result}ms`;
      $screen.classList.replace("now", "waiting");
      $screen.textContent = "클릭해서 시작하세요.";
    }
  });
  ```

  1. `let`은 *블록 스코프*다. 위와 같이 조건분기 또는 특정 메서드 내에 선언을 하게 되면 그 블록 내에서만 유효하다.<br>
     그럼 모든걸 감싸고 있는 `addEventListener`함수 내에 선언을 해보자.

     ```javascript
     $screen.addEventListener("click", (event) => {
       let startTime; // 선언
       let endTime; // 선언
       if (event.target.classList.contains("waiting")) {
         $screen.classList.remove("waiting");
         $screen.classList.add("ready");
         $screen.textContent = "초록색이 되면 클릭하세요";
         setTimeout(() => {
           startTime = new Date(); //호출
           $screen.classList.remove("ready");
           $screen.classList.add("now");
           $screen.textContent = "클릭하세요!";
         }, Math.floor(Math.random() * 1000) + 2000);
       } else if (event.target.classList.contains("ready")) {
       } else if (event.target.classList.contains("now")) {
         endTime = new Date(); //호출
         result = endTime - startTime;
         $result.textContent = `${result}ms`;
         $screen.classList.replace("now", "waiting");
         $screen.textContent = "클릭해서 시작하세요.";
       }
     });
     ```

  2. 함수 내에 정의를 하면 `addEventListener`가 종료될 때 변수가 지워지므로 첫번째 클릭 후에 변수 `startTime`값이 지워지고 두번째 클릭을 하게 되면 `startTime`값이 없는 상황이므로 `result`값이 원하는 값으로 나오지 않는다.

     ```javascript
     let startTime; // 선언
     let endTime; // 선언
     $screen.addEventListener("click", (event) => {
       if (event.target.classList.contains("waiting")) {
         $screen.classList.remove("waiting");
         $screen.classList.add("ready");
         $screen.textContent = "초록색이 되면 클릭하세요";
         setTimeout(() => {
           startTime = new Date(); //호출
           $screen.classList.remove("ready");
           $screen.classList.add("now");
           $screen.textContent = "클릭하세요!";
         }, Math.floor(Math.random() * 1000) + 2000);
       } else if (event.target.classList.contains("ready")) {
       } else if (event.target.classList.contains("now")) {
         endTime = new Date(); //호출
         result = endTime - startTime;
         $result.textContent = `${result}ms`;
         $screen.classList.replace("now", "waiting");
         $screen.textContent = "클릭해서 시작하세요.";
       }
     });
     ```

  3. 위와 같이 바깥에 **전역선언**을 하면 원하는 결과값이 나오는 것을 볼 수 있다.

- ## **`const`**

```javascript
const a = "123";
a = "456";

//result
//Uncaught TypeError: Assignment to constant variable...
```

```javascript
const array = [1, 2, 3];
array.push(4);
console.log(`${array}`);

// result
// [1,2,3,4]

array = [1, 3, 5];

//result
//Uncaught TypeError: Assignment to constant variable...
```

`const`를 사용하면 해당 변수를 대입할 수 없다.<br>
그러므로 `const 변수명 = 초기값;`으로 했을 때, 아래에 `변수명 = 대입값;`이라는 단순 참조를 할 수 없다.

---

- ## **`그래서 스코프가 뭐야?`**

  ES6(ES2015)이전에는 변수를 선언할 수 있는 키워드가 `var`뿐이었다. <br>
  하지만 ES6에서는 `let`,` const` 키워드가 추가되어 이를 이용해서도 변수를 선언할 수 있게 됐다.<br>

**var, let, const의 차이**

1. `var`는 <u>함수 레벨 스코프</u>이고 `let`,`const`는 <u>블록 레벨 스코프</u><br>
2. `var`로 선언한 변수는 선언 전에 사용해도 에러가 나지 않지만 `let`, `const`는 에러 발생.<br>
3. `var`는 이미 선언되어있는 이름과 같은 이름으로 변수를 또 선언해도 에러가 나지 않지만 `let`, `const`는 이미 존재하는 변수와 같은 이름의 변수를 또 선언하면 에러 발생.<br>
4. `var`, `let`은 변수 선언시 초기 값을 주지 않아도 되지만 `const`는 반드시 초기값 할당 필요.<br>
5. `var`, `let`은 값을 다시 할당할 수 있지만 `const`는 한번 할당한 값은 변경할 수 없다.(단, 객체 내부의 프로퍼티가 변경되는 것까지 막지는 못한다).<br>

2번의 경우를 보면 ‘그럼 `var`는 호이스팅이 되고 `let`, `const`는 호이스팅이 안되기 때문에 그러는건가?’ 라고 생각할 수 있는데, 그렇지 않다.
<br>자바스크립트에서 사용하는 `var, let, const, function, class` 등의 선언 키워드들은 모두 호이스팅이 된다.<br>
`var`의 경우 호이스팅되면서 초기 값이 없으면 자동으로 `undefined`를 초기값으로 하여 메모리를 할당한다. 그래서 `var`의 경우 선언 전에 해당 변수를 사용하려고 해도 메모리에 해당 변수가 존재하기 때문에 에러가 발생하지 않는다.

```javascript
console.log(num); // undefined
var num;
```

그런데 `let, const`의 경우 호이스팅이 되면서 초기 값이 없다면 `var`처럼 자동으로 초기값을 할당하지 않는다. (`const`의 경우 선언시 초기값을 할당하지 않으면 Syntax에러 발생). 그래서 값이 할당되기 전까지 메모리를 할당하지 않기 때문에 선언 전에 (정확히는 값이 할당되기 전에) 사용하려고 하면 메모리에 해당 변수가 존재하지 않아 에러를 발생시킨다. <br>
이처럼 <u>변수가 선언되고 해당 변수에 값이 할당되기 전까지</u>를 `TDZ(Temporal Dead Zone)`라고 한다.

```javascript
console.log(num); // Uncaught ReferenceError: num is not defined
let num = 1;
console.log(num); // 1

// 호이스팅 되었을 때
let num;
console.log(num); // 값이 할당되기 전이므로 아직 num은 메모리에 존재하지 않음. TDZ.
num = 1;
console.log(num); // 1
```

5번의 경우 `const`로 선언한 변수에 객체를 할당했을 때 `const` 변수가 가리키고 있는 <u>객체 자체를 변경하는 것은 불가능</u>하지만 <u>해당 객체 내의 프로퍼티가 변경되는 것까지 막는다는 것은 아니다.</u> 아래의 코드를 보자.

```javascript
const obj = { first: 1 };

// 에러 발생
obj = 1; // Uncaught TypeError: Assignment to constant variable.

// 에러 발생하지 않음.
obj.first = 2;
obj.second = 2;
delete obj.first;
```

위와 같이 <u>`const` 변수가 가리키는 값 자체</u>를 변경하려고 하면 에러가 발생하지만 객체 내의 프로퍼티의 <u>추가, 변경, 삭제</u>를 하는 것은 문제가 없다.
만약 객체의 프로퍼티도 변경되지 않게 객체를 선언하고 싶으면 `Object.freeze()` 메소드를 사용할 수 있다(Object.freeze()는 얕은 동결이기 때문에 객체 내의 또다른 객체의 프로퍼티 변경까지 동결하지는 못한다).<br>
결론: `var`의 경우 버그 발생과 메모리 누수의 위험 등이 있기 때문에 `var`말고 `let`, `const`를 사용하는 것이 좋다.

---

- #### **reduce**

  ```javascript
  [1, 2, 3, 4].reduce((a, c) => a + c, 0);

  // a는 누적값, c는 현재값, a+c는 다음 누적값, 0은 초기값.
  // a:0 c:1
  // a:1 c:2
  // a:3 c:3
  // a:6 c:4
  // 10
  ```

  - 위와 같이 `배열명.reduce((누적값인자, 현재값인자, 다음 인덱스) => (리턴하는 다음 누적값), 초기값);` 형식으로 작성할 수 있으며, 뜻 그대로 여러개의 배열요소를 하나의 값으로 **축소**해주는 역할을 한다.
    - 정석은 아래와 같다.
      ```javascript
      배열. reduce((누적값,현재값)=>{
        return 새로운 누적값;
      }, 초기값);
      ```
  - 그래서 평균을 구하고 싶으면 `[1,2,3,4].reduce((a,c)=>(a+c),0) / [1,2,3,4].length` (누적값/배열길이)로 표현이 가능하다.
  - 초기값이 없으면 배열 0 index의 1이 초깃값이 된다. 초깃값은 첫 번째 누적값으로 들어가기 때문에 이때는 두 번째 요소부터 `reduce`를 적용하게 된다. 따라서 누적값(a) 1, 현재값(c) 2인 상태로 함수가 시작된다. 반환값인 3은 다음 번의 누적값이 된다.
  - 또한 이렇게도 가능하다.

    ```javascript
    ['철수','영희','민초','하와이안'].reduce((a,c,i)=>{a[i]=c; return a},{});

    result
    {0:'철수', 1:'영희', 2: '민초', 3:'하와이안'}

    a:{}, c:'철수', i:0
    a:{0:'철수'}, c:'영희', i:1
    a:{0:'철수', 1:'영희'}, c:'민초', i:2
    a:{0:'철수', 1:'영희', 2:'민초'}, c:'하와이안', i:3
    a:{0:'철수', 1:'영희', 2:'민초', 3:'하와이안'}
    ```

    - 위와 같이 `reduce`로 객체 리터럴 생성이 가능한 것을 볼 수 있다. 중요한건 `초기값`을 무엇으로 했는지 잘 봐야한다. <u>map과 reduce는 가장 중요한 개념이기에 반드시 숙지해야 한다.</u> + `실행컨텍스트`와 `promise`, `이벤트루프`만 마스터하자.

---

- ### `[일지]` 프론트엔드 개발자가 되기로 한 이유와 고찰
  - 자바스크립트의 특징: 범용성과 확장성.
  - 생명주기
    - 앞으로 10년 뒤에 세상은 아마 네이티브로 남아있을 수 밖에 없는 몇 안되는 앱을 제외하고는 모두 웹 기반 앱으로 이주하거나 새로운 웹 앱이 생겨날 것 같다. 웹 기반 앱의 가장 큰 단점은 성능인데, 브라우저 및 유저환경의 성능이 계속 향상되면 일반 네이티브 앱 만큼 성능을 낼 수 있도록 빠르게 동작할 것이라 예상한다. 그러면 굳이 네이티브로 만들 필요 없이 웹 기반으로 개발하는 것이 당연하게 될 것 같다.
  - 트렌드의 변화
    - 물론 다른 개발스택도 트렌드는 존재한다. 하지만 강력한 범용성으로 인하여 비즈니스에서도 이 웹개발에 대한 흥미도가 올라가고, 그에 따라 인재풀이 급격하게 증가하여 웹의 수요와 공급이 증가하고 있는 상황이다. 근데 이 스택 마저도 언제까지 이어질진 모르겠으나 T-Leaning의 원칙에 입각하여 급증하고 있는 시장 속에 방대한 정보를 활용해 트랜드를 파악하면서 하나만 파고 지적 욕구를 채워나가기 위함이 이유 중 하나다.
