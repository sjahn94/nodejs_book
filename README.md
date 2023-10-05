# nodejs_book
백견불여일타 Node.js로 서버 만들기 내용 정리

# Chapter 01: Node.js 첫걸음
- Node.js란, 자바스크립트 런타임을 의미하기도 하지만, Node.js만의 API를 의미하기도 한다.
- Node.js는 논블로킹/비동기(NonBlocking - Async) 처리 방식을 사용한다.
- 논블로킹/비동기는 작업의 흐름이 순차적이지 않고, 응답을 가디리지 않고, 바로 다음 작업을 실행해버리는 것을 말한다. 웹 서버와 같이 수많은 요청이 들어와 처리해야 하는 작업을 블로킹/동기(Blocking - Sync) 방식으로 사용한다면 어떤 작업에 대한 완료 응답이 올때까지 기다리면서 자원을 낭비한다.
- Node.js는 싱글 스레드를 사용하지만 비동기 방식으로 작업을 처리 할 수 있는 이유는 이벤트 루프가 있기 때문이다.
- 이벤트 루프는 어떤 이벤트가 있는지 감시하는 감시자이고 이벤트가 감지하면 그 작업을 위해 작성 스레드를 생성해 그 이벤트 처리를 맡긴다.
