# nodejs_book
백견불여일타 Node.js로 서버 만들기 내용 정리

# Chapter 01: Node.js 첫걸음
- Node.js란, 자바스크립트 런타임을 의미하기도 하지만, Node.js만의 API를 의미하기도 한다.
- Node.js는 논블로킹/비동기(NonBlocking - Async) 처리 방식을 사용한다.
- 논블로킹/비동기는 작업의 흐름이 순차적이지 않고, 응답을 가디리지 않고, 바로 다음 작업을 실행해버리는 것을 말한다. 웹 서버와 같이 수많은 요청이 들어와 처리해야 하는 작업을 블로킹/동기(Blocking - Sync) 방식으로 사용한다면 어떤 작업에 대한 완료 응답이 올때까지 기다리면서 자원을 낭비한다.
- Node.js는 싱글 스레드를 사용하지만 비동기 방식으로 작업을 처리 할 수 있는 이유는 이벤트 루프가 있기 때문이다.
- 이벤트 루프는 어떤 이벤트가 있는지 감시하는 감시자이고 이벤트가 감지하면 그 작업을 위해 작성 스레드를 생성해 그 이벤트 처리를 맡긴다.

콜백 큐 : 이벤트 발생 후 호출되어야 할 작업을 기다리는 자료구조, 이벤트 큐라고도 한다.

콜 스택 : 함수의 호출을 기록하는 자료구조이며, 현재 실행 중인 작업이 끝났을 때 어느 실행 부분으로 돌아갈지 보관한다.

# Chapter 02: 자바스크립트 리마인드
변수, 호이스팅, 클로저

<b>변수 호이스팅</b>
```
console.log(puppy); // 실행결과 : undefined
var puppy = "cute";
console.log(puppy); // 실행결과 : cute
```
puppy라는 변수를 아직 선언하지 않은 상태에서 호출하였는데 오류가 발생하지 않고 undefined라는 값을 반환한다. 이런 현상을 바로 '변수 호이스팅'이라고 한다. 

<b>let을 사용한 변수 호이스팅 문제 해결</b>
```
let dog;
dog = "happy";
console.log(dog); // 실행결과 : happy
let dog = "happy"; // Identifier 'dog' has already been declared
```

<b>const를 사용한 변수 호이스팅 문제 해결</b>
```
const puppy = "cute";
const puppy = "so cute"; // 'puppy' has already been declared
```

<b>클로저의 개념</b>
```
function outer(){
  var a = 'A';
  var b = 'B';
  function inner(){
    var a = 'AA';
    console.log(b);
  }
  return inner;
}

var outerFunc = outer();
outerFunc(); // B
```
클로저(closure)는 내부 함수가 외부 함수의 스코프(범위)에 접근할 수 있는 것을 말한다.


<b>객체와 배열</b><br>
자바스크립트에서 객체는 키(key)와 값(value)의 쌍으로 이루어진 프로터피의 정렬되지 않은 집합을 의미한다
```
const country = {
  name: "Korea",
  population: "5178579",
  get_name: function(){
    return this.name;
  }
};
```

<b>객체 배열 생성</b><br>
```
const coffee = [];

coffee.push({ name: 'Americano'});
coffee.push({ name: 'Lattee'});

console.log(coffee); // [ {name: 'Americano'}, { name: 'Latte' }]
console.log(coffee[0]); // { name: 'Americano' }
console.log(coffee.length); // 2
```

<b>구조 분해 할당</b><br>
```
const animal = ['dog', 'cat'];

let [first, second] = animal;

console.log(first); // dog
console.log(second); // cat
```
