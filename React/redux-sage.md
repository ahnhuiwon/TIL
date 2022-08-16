## `Redux-saga`

### `Redux-saga란?`

이전에 정리한 redux-thunk는 함수형태의 액션을 디스패치하여 미들웨어에서 해당 함수에

스토어의 디스패치와 getState를 파타미터로 넣어서 사용하는 원리이다. 대부분의 경우에는 redux-thunk로도 충분히 구현할 수 있으나

redux-saga는 조금 더 까다로운 상황에서 유용하다. 예를 들어 아래와 같은 상황에서는 유리하게 사용할 수 있다.

  - 기존 요청을 취소 처리해야 할 때(불필요한 중복 요청 방지)
  - 특정 액션이 발생했을때 다른 액션을 발생시키거나, API 요청 등 리덕스와 관계 없는 코드를 실행할 때
  - 웹 소켓을 사용할 때
  - API 요청 실패 시 재요청해야 할 떄

<br />

### `제너레이터 함수란?`

redux-saga에서는 ES6의 제너레이터 함수라는 문법을 사용한다.

이 문법의 핵심은 **함수를 특정 구간에서 멈춰놓을 수 있고, 원할 때 다시 돌아가게 할 수 있다**

아래와 같은 함수가 있다고 가정하자.

```

function temp_func(){
  return 1;
  return 2;
  return 3;
}

```

<br />

하나의 함수에서 값을 여러개 반환하는 것은 불가능 하기때문에 이 코드는 제대로 작동하지 않는다.

결과값으로는 맨 위의 값인 1이 반환된다.

그렇다면 아래 코드를 작성해보자.

```

function* generator_func(){
  console.log('안녕하세요');
  yield 1;
  console.log('제너레이터 함수');
  yield 2;
  console.log('function*');
  yield 3;
  return 4;
}

```

<br />

제너레이터 함수를 만들 때는 function* 키워드를 사용한다.

함수를 작성한 뒤 아래 코드를 사용해 제너레이터를 생성한다.

const generator = generator_func();

제너레이터 함수를 호출했을때 반환되는 객체를 **제너레이터**라고 부른다.

위 코드를 next()로 호출하면 다음과 같은 결과를 출력한다.

<br />

![스크린샷, 2022-08-16 23-11-20](https://user-images.githubusercontent.com/94499416/184902051-0a830c6c-dfcd-4570-8ecb-2b763e74cecc.png)

<br />

제너레이터는 처음 만들어지면 함수의 흐름은 멈춰 있는 상태이다.

next()를 호출하면 다음 yield가 있는 곳까지 호출하고 다시 함수는 멈춘다.

즉 제너레이터 함수를 사용하면 함수를 도중에 멈출 수도 있고, 순차적으로 여러 값을 반환시킬 수도 있다.

또한 next 함수에 파라미터를 넣으면 제너레이터 함수에서 yield를 사용해 해당값을 조회할 수도 있다.

<br />

```

function* sum_generator(){
    console.log('run');
    let a = yield;
    let b = yield;
    yield a+b;
}

```

<br />

redux-saga는 제너레이터 함수 문법을 기반으로 비동기 작업을 관리해준다.

즉 우리가 디스패치하는 액션을 모니터링하여 그에 따라 필요한 작업을 따로 수행할 수 있는 미들웨어이다.

아래 코드는 redux-saga와 비슷한 코드로 작동하는 코드이다.

<br />

```

function* watch_gener(){
    console.log('watch...');
    let prev_action = null;
    while(true){
        const action = yield;
        console.log('prev_action : ', prev_action);
        prev_action = action;
        if(action.type === 'MATCH'){
            console.log('일치');
        }
    }
}

``
<br />

![스크린샷, 2022-08-16 23-50-49](https://user-images.githubusercontent.com/94499416/184910498-8ccda2ee-7fa4-49a9-a85d-a51f401ccb85.png)

<br />
