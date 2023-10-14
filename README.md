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

<b>함수</b><br>
```
function add(a,b){
  return a+b;
}
console.log(add(1,4)); // 5
```

<b>화살표 함수</b><br>
```
const add = (a,b) => {
  return a+b;
}
console.log(add(1,4)); // 5
```

화살표 함수에는 함수명, arguments, this, 이 세 가지가 없다는 점이 특징이다. 함수명이 없다는 것은 익명 함수로 동작한다는 뜻이고 arguments, this가 없다면 다음과 같은 일이 발생한다.
```
const func = function(){
  console.log(arguments);
}
function(1,2,3,4); // [Arguments] {'0':1, '1':2, '2':3, '3':4}
```

```
const func = (...args) => {
  console.log(args);
}

func(1,2,3,4); // [1,2,3,4]
```

화살표 함수에는 arguments가 자동으로 생성되지 않기 때문에 arguments가 필요하다면 함수의 파라미터 부분에 ...args를 넣어 args라는 배열 안에 파라미터를 담을 수 있다.
...은 전개 연산자라고 하는데, "값이 몇 개가 될지 모르나 뒤에 오는 변수명에 값을 배열로 넣어라"라고 하는 의미이다.

<b>콜백 함수</b><br>
콜백(Callback)은 나중에 실행되는 코드를 의미한다. 즉 다른 함수에 매개변수로 넘겨준 함수를 의미하며 
매개변수로 넘겨받은 함수는 일단 넘겨받고, 때가 되면 나중에 호출(called back)한다는 것이 콜백함수의 개념이다.

```
setTimeout(() => {
  console.log('todo: First work!');
}, 3000);

setTimeout(() => {
  console.log('todo: Second work!');
},2000);
```
// todo : Second work!
// todo : First work!

setTimeout() 함수는 콜백 함수와 지체할 시간을 인자로 받아 인자로 받은 시간만큼 기다렸다가 콜백 함수를 실행하는 함수이다. 자바스크립트는 이벤트 중심 언어이기 때문에 어떤 이벤트가 발생하고, 그에 대한 결과가 올 때까지 기다리지 않고 다음 이벤트를 계속 실행 한다.

<b>Promise</b><br>
Promise는 코드의 중첩이 많아지는 콜백 지옥을 벗어날 수 있게 해주는 객체이다.

```
function workP(sec){
  // Promise의 인스턴스를 반환하고
  // then에서 반환한 것을 받는다.
  return new Promise((resolve, reject) => {
    // Promise 생성시 넘기는 callback = resolve, reject
    // resolve 동작 완료시 호출, 오류 났을 경우 reject
    setTimeout(() => {
      resolve(new Date().toISOString());
    }, sec * 1000);
  }); 
}

workP(1).then((result) => {
  console.log('첫 번째 작업', result);
  return workP(1);
}).then((result) => {
  console.log('두 번째 작업', result);
});
```

# Chapter 03: 5줄로 만드는 서버
<b>package.json</b><br>
package.json 파일은 프로젝트에 들어 있는 여러 패키지 정보를 관리해주는 파일 이다.

<b>npm init</b><br>
npm init 명령어는 Node.js를 시작하기 위한 초기화(initial) 작업이자 package.json을 만드는 명령어이다.

package name -> 패키지명<br>
entry point -> 실행 파일의 진입점 역할을 하는 파일을 지정, 주로 index.js, app.js, server.js등의 이름을 사용하며 기본값은 index.js이다.<br>
test command -> 코드를 테스트할 때 입력할 명령어<br>
keyword -> npm에서 패키지를 찾게 해주는 옵션<br>
license -> 원하는 라이선스를 지정해주는 옵션<br>

<b>모듈 시스템</b><br>
모듈이란 기능 단위로 분리하고 기능을 이루는 코드를 모아서 캡슐화해 놓은 것을 '모듈'이라고 한다.<br>
Require() : 모듈을 불러온다<br>
Module.exports=프로퍼티 또는 exports.프로퍼티 = 모듈을 내보낸다.

<table>
  <th>구분</th>
  <th>정의 방식</th>
  <th>require()의 호출 결과</th>
  <tr>
    <td>exports</td>
    <td>값 자체를 할당하는 것이 아닌, 외부로 보낼 요소를 exports 객체의 프로퍼티 또는 메서드로 추가합니다.</td>
    <td>프로퍼티와 메서드가 담긴 exports 객체를 require()로 받게 된다.</td>
  </tr>
  <tr>
    <td>module,exports</td>
    <td>객체에 하나의 값(원시 타입, 함수, 객체)만 할 당할 수 있다.</td>
    <td>module.exports 객체에 할당된 값 자체를 require()를 통해 받는다.</td>
  </tr>
</table>

<b>http 모듈</b><br>
http.createServer() : 서버를 만드는 함수<br>
res.writeHead() : 응답에 대한 정보(헤더)를 기록하는 함수<br>
res.write() : 클라이언트에 보낼 데이터<br>
res.end() : 응답을 종료하는 메서드<br>
fs.readFile() : 파일의 내용을 읽는다<br>

<b>express 모둘</b><br>
Node.js의 핵심 모듈인 http, Connect 컴포넌트를 기반으로 하는 웹 프레임워크
```
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(8080, () => {
    console.log('8080포트에서 서 실행중');
});
```
res.send() : 클라이언트에 보낼 데이터<br>
```
app.get('/', (req, res) => {
    res.sendFile(__dirname + '/index.html');
});
```
res.sendFile() : 클라이언트에 해당 파일의 내용을 보내주는 메소드<br>

nodemon : 파일들을 감시하고 있다가 Node.js 소스 수정 시 자동으로 서버를 재시작 해주는 모듈

<b>express와 미들웨어</b><br>
미들웨어는 중간 작업을 해주는 역할을 한다. 즉, 요청과 응답 사이에 express 자체에 있는 기능 외에 추가적인 기능을 넣어줄 수 있다.<br>
app.use() 메서드를 통해 사용하고 app.set()과의 차이점은 app.set()은 전역으로 사용된다.

```
const express = require('express');
const app = express();

app.get('/', function(req, res, next){
    res.send('Hello World!');
    next();
});

const myLogger = function(req, res, next){
    console.log('LOGGED');
    next();
};

app.use(myLogger);

app.listen(8080);
```
localhost:8080/에 접속 하였을때 myLogger를 반드시 거치게 된다. next()는 다음 미들웨어로 넘어가는 역할을 한다.<br>

<table>
  <th>종류</th>
  <th>내용</th>
  <tr>
    <td>next()</td>
    <td>다음 미들웨어로 가는 역할을 한다.</td>
  </tr>
  <tr>
    <td>next(error)</td>
    <td>오류 처리 미들웨어로 가는 역할을 한다.</td>
  </tr>
</table>

<b>express.static</b><br>
static 파일(정적 파일) : 이미지, css, 스크립트 파일과 같이 그 내용이 고정되어 있어, 응답을 할 때 별도의 처리 없이 파일 내용 그대로를 보여주면 되는 파일
```
const express = require('express');

const app = express();
app.set('port', process.env.PORT || 8080);

app.use(express.static(__dirname + '/public'));

app.get('/', (req, res) => {
    res.sendFile(__dirname + '/index2.html');
});

app.listen(app.get('port'), () => {
    console.log(app.get('port'), '번 포트에서 서버 실행 중..');
});
```

<b>router</b><br>
router도 일종의 미들웨어이며, 클라이언트로부터 요청(request)이 왔을 때 서버에서 어떤 응답(response)을 보내주어야 할지 결정해준다.
<table>
  <th>종류</th>
  <th>주소</th>
  <th>응답</th>
  <tr>
    <td>GET</td>
    <td>/</td>
    <td>index.html 파일을 송신한다.</td>
  </tr>
  <tr>
    <td>GET</td>
    <td>/join</td>
    <td>join.html 파일을 송신한다.</td>
  </tr>
  <tr>
    <td>POST</td>
    <td>/user</td>
    <td>사용자를 등록한다.</td>
  </tr>
  <tr>
    <td>PUT</td>
    <td>/user/user_id</td>
    <td>user_id를 가진 사용자의 정보를 수정한다.</td>
  </tr>
  <tr>
    <td>DELETE</td>
    <td>/user/user_id</td>
    <td>user_id를 가진 사용자의 정보를 삭제한다.</td>
  </tr>
</table>

app.use('/경로', 미들웨어);<br>
app.get('/경로', 미들웨어);<br>
app.post('/경로', 미들웨어);<br>
app.put('/경로', 미들웨어);<br>
app.delete('/경로', 미들웨어);<br>

```
app.get("/user/:id", function(req, res) {
  res.send("user id: " + req.params.id);
});
```

<b>cookie-parser</b><br>
Node.js에서 writeHead 함수에 Set-Cookie 설정 값 을 설정하여 쿠키를 생성할 수 있다.
```
const http = require('http');

http.createServer((req, res) => {
    res.writeHead(200, {'Set-cookie': 'name=roadbook'});
    console.log(req.headers.cookie);
    res.end('Cookie ---> Header');
}).listen(8080, () => {
    console.log('8080포트에서 서버 연결 중 ..');
});
```
클라이언트가 쿠키와 함께 요청을 보내면 rea.headers.cookie를 통해 쿠키 값에 접근 할 수 있다. req.headers.cookie에 저장된 값은 문자열인데<br>
이를 자바스크립트에서 사용하기 위해서는 객처로 파싱하는 과정이 필요하다. Cookie-parser없이 파싱하려면 따로 파싱하는 함수를 만들어야한다.<br>

<b>세션 생성</b>
```
const http = require('http');

const session = {};
const sessKey = new Date();
session[sessKey] = { name: 'roadbook' };

http.createServer((req, res) => {
    res.writeHead(200, { 'Set-cookie': `session=${sessKey}`});
    res.end('Session-Cookie --> Header');
}).listen(8080, () => {
    console.log('8080포트에서 서버 연결 중..');
});
```
