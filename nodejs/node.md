노드는 자바스크립트 런타임이다. 

런타임이란?

> 특정 언어로 만든 프로그램들을 실행할 수 있는 환경
> 

따라서 노드를 이용하면 자바스크립트로 구성된 프로그램을 컴퓨터에서 실행할 수 있는데

쉽게 말해 노드는 자바스크립트 실행기라고 봐도 무방하다.

기존에는 브라우저에서 자바스크립트 런타임을 내장하고 있어 자바스크립트 코드를 실행 할 수 있었고, 브라우저 외의 환경에서는 자바스크립트의 실행 속도에 문제 때문에 좋은 

평가를 받지 못했다.

2008년 구글이 V8 엔진을 사용한 크롬을 출시하자, V8엔진은 다른 자바스크립트 엔진과

달리 매우 빨랐고, 오픈 소스로 코드를 공개했다.

![node_in.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ece6007-b55a-4259-97ee-2846bcad2166/node_in.png)

노드는 V8과 더불어 libuv라는 라이브러리를 사용하는데,  V8과 libuv는 C와 C++로

구현되어 있지만 노드가 알아서 V8과 libuv에 연결해주기 때문에 노드를 사용할 때

C와 C++은 몰라도 전혀 지장이 없다. 

libuv 라이브러리는 노드의 특성인 **이벤트 기반**과 **논 블로킹 I/O 모델**을 구현하고 있다.

---

## 이벤트 기반이란?

> 이벤트가 발생할 때 미리 지정해둔 작업을 수행하는 방식
> 

이벤트 기반 시스템에서는 특정 이벤트가 발생할 때 무엇을 할지 미리 등록해두어야 한다.

위와 같은 행위를 이벤트리스너에 콜백 함수를 등록한다고 표현하는데 예를 들어

1. 클릭 이벤트 리스너에 경고창을 띄우는 콜백 함수를 등록
2. 클릭 이벤트가 발생할 때마다 콜백 함수가 실행
3. 경고창 출력

노드도 이벤트 기반 방식으로 동작하기 때문에, 이벤트가 발생하면 이벤트 리스너에

등록해둔 콜백 함수를 호출하고 발생한 이벤트가 없거나, 이벤트를 다 처리하면

노드는 다음 이벤트가 발생할 때까지 대기한다.

![event_build.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/edb73e91-00c5-4bcf-ba95-f6d721f4da5d/event_build.png)

이벤트 기반 모델에서는 이벤트 루프라는 개념이 등장하는데 여러 이벤트가

동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지를 이벤트 루프가 판단한다.

이벤트 루프

노드는 자바스크립트 코드의 맨 위부터 한 줄씩 실행한다.

함수 호출 부분을 발견했다면 호출한 함수를 호출 스택에 넣는다.

아래 코드를 한번 보자

```jsx
function first() {
	second();
	console.log('1');
}

function second() {
	third();
	console.log('2');
}

function third() {
	console.log('3');
}

first();
```

first()가 제일 먼저 호출되고 그 안에서 second() 함수가 호출된 뒤,

마지막으로 third() 함수가 호출된다. 실행은 호출된 순서와 반대로 실행이 완료된다.

![call_stack.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a15fb16-be70-4fb4-8c84-b2c8658edefd/call_stack.png)

참고로 anonymous 함수는 처음 실행 시 전역 컨텍스트를 의미한다.

컨텍스트는 함수가 호출되었을 때 생성되는 환경을 의미한다.

자바스크립트는 실행 시 기본적으로 전역 컨텍스트 안에서 돌아가는데 함수가 실행되는

동안 호출 스택에 머물러 있다가 실행이 완료되면 호출 스택에서 지워진다.

위 코드에서는 third(), second(), first() , anonymous() 순서로 지워지고

anonymous 컨텍스트까지 실행이 모두 완료되었다면 호출 스택은 비어 있게 된다.

![func_1.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aca76554-fe72-436c-8ea6-69571d65ecb5/func_1.png)

이번에는 3초 이후에 코드를 실행하는 setTimeout을 사용하여

콘솔에 어떤 로그가 기록될지 예측해보자.

```jsx
function run() {
	console.log('3초 후 실행');
}

console.log('시작');
setTimeout(run, 3000);
console.log('끝');
```

![11111111.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e11d08ac-0960-4d61-bed3-5cdd0b7e6991/11111111.png)

콘솔 결과를 예측하기엔 쉽지만, 호출 스택으로는 설명하기가 어렵다. setTimeout 함수의 콜백인 run()이 호출 스택에 언제 들어가는지 쉽게 파악할 수 없기 때문이다.

- 이벤트 루프 : 이벤트 발생 시 호출할 콜백 함수들을 관리, 호출된 콜백 함수의 실행 순서를 결정
- 백그라운드 : setTimeout 같은 타이머 및 이벤트 리스너들의 대기 장소
- 태스크 큐 : 이벤트 발생 후, 백그라운드에서 타이머 또는 이벤트 리스너의 콜백 함수를 보내는 장소

아래 그림을 참고하면 이해하기 좀 더 편할 수 있다.

![loop_first.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/41639f79-5f04-49a3-a331-c725c1dba5a4/loop_first.jpg)

![loop_second.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6c4b0eb-be48-422c-90ef-6b423996508f/loop_second.jpg)

![loop_third.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1c7b4f5-43ad-4a9e-9d84-47400546d941/loop_third.jpg)

이벤트 루프는 호출 스택이 비어 있을 때만 태스크 큐에 있는 함수를 호출 스택으로

보내는 특성이 있다. 

만약 4번에 과정에서 호출 스택에 함수들이 너무 많이 들어 있다면?

3초로 설정은 했지만 3초가 지난 후에도 run 함수가 실행되지 않을 수도 있다.

이것이 호출 스택으로 설명이 어려운것과 동시에 setTimeout의 시간이 정확하지 않을 수도 있는 이유이다.

---

## 논 블로킹 I/O

I/O는 입력(input)/출력(output)을 의미한다.

논 블로킹은 이전 작업이 완료될 때가지 대기하지 않고 다음 작업을 수행하는 것을 뜻한다.

> 입력과 출력 작업이 완료될 때까지 대기하지 않고 다음 작업을 수행하는 것
> 

노드는 I/O 작업을 백그라운드로 넘겨 동시에 처리하는데 동시에 처리할 수 있는

작업들을 최대한 묶어서 백그라운드로 넘겨야 시간을 절약할 수 있다.

![non_blocking.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f045996f-1426-4ab4-a19d-3d1fe39358d3/non_blocking.jpg)

아래는 블로킹 방식의 코드이다.

```jsx
function long_running() {
	// 오래 걸리는 코드 작업들
	console.log('작업 끝');
}

console.log('시작');
long_running();
console.log('다음 작업');
```

결과는 다음과 같다.

![스크린샷, 2022-08-27 16-13-30.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81353639-9445-477a-8d25-47d89c6daf40/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-08-27_16-13-30.png)

long_running 이라는 작업이 완료되기 전까지는 console.log(’다음 작업’)이 호출되지 않는다.

이번에는 setTimeout을 사용하여 코드를 변경해보자.

```jsx
function long_running() {
	// 오래 걸리는 코드 작업들
	console.log('작업 끝');
}

console.log('시작');
setTimeout(long_running, 0);
console.log('다음 작업')
```

결과는 다음과 같다.

![스크린샷, 2022-08-27 16-20-59.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/567dbce0-94b3-4fa4-92b2-eddf5dc5ba1a/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2022-08-27_16-20-59.png)

이 setTimeout(콜백, 0)은 코드를 논 블로킹으로 만들기 위해 사용하는 기법 중 하나로

이벤트 루프를 이해했다면 setTimeout의 콜백 함수인 long_running은  태스크 큐로 보내져 순서대로 실행되지 않는다는 것을 알 수 있다.

<aside>
⚠️ setTimeout(콜백, 0)은 바로 실행되는 것이 아니다.
브라우저와 노드에서는 기본적인 지연 시간이 있어 바로 실행되지 않는다.

</aside>

---

## 싱글 스레드

자바스크립트 코드는 동시에 실행될 수 없는 이유, 다만 노드는 엄밀히 따지자면 

싱글 스레드로 동작하지는 않다.

[konw_cs_frontend/Process.md at develop · ahnhuiwon/konw_cs_frontend](https://github.com/ahnhuiwon/konw_cs_frontend/blob/develop/md_folder/cs/Process.md)

노드를 실행하면 프로세스가 하나 생성이 되고, 그 프로세스에서 스레드를 생성하는데,

이때 내부적으로 여러 스레드를 생성한다.

그중에서도 직접 제어할 수 있는 스레드는 하나뿐인데 이로 인해 노드가 싱글 스레드라고 여겨지는 이유이다.

아래는 노드의 장단점을 정리한 표이다.

| 장점 | 단점 |
| --- | --- |
| 멀티 스레드 방식에 비해 적은 컴퓨터 자원 사용 | 기본적으로 싱글 스레드라서 CPU 코어를 하나만 사용 |
| I/O 작업이 많은 서버로 적합 | CPU 작업이 많은 서버로는 부적합 |
| 멀티 스레드 방식보다 쉬움 | 하나뿐인 스레드가 멈추지 않도록 관리가 필요함 |
| 웹 서버가 내장되어 있음 | 서버 규모가 커졌을 때 서버를 관리하기 어려움 |
| 자바스크립트를 사용함 | 어중간한 성능 |
| JSON 형식과 쉽게 호환됨 |  |
