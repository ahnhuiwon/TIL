## `Redux-Middleware`

### `왜 Redux-Middleware인가?`

리액트트 웹 어플리케이션에서 API 서버를 연동할 때에는 API 요청애 대한 상태도 잘 관리해야한다.

예를 들어 요청이 시작되었을 때는 로딩중임을, 요청이 성공하거나 실패했을때는 로딩이 끝났음을 명시해야한다.

요청이 성공하면 서버에서 받아 온 응답에 대한 상태를 관리하고, 요청이 실패하면 서버에서 반환한 에러에 대한

상태를 관리해야 한다.

리액트 프로젝트에서 리덕스를 사용하고 있으면서 이러한 비동기 작업을 관리해야한다면 미들웨어를 사용하여

매우 효율적이고 편하게 상태 관리를 할 수 있다.

<br />

### `Middleware란?`

리덕스 미들웨어는 액션을 디스패치 했을 때 리듀서에서 이를 처리하기에 앞서 사전에 지정된

작업들을 실행한다. 즉 미들웨어는 액션과 리듀서 사이의 중간자라고 볼 수 있다.
  > 예를 들어 <br />
  > 전달받은 액션을 단순히 콘솔에 기록. <br />
  > 전달받은 액션 정보를 기반으로 액션을 아예 취소하는 작업

대표적으로 redux-thunk와 redux-saga가 있는데 이 글에서는 redux-thunk를 먼저 다뤄볼 예정이다.

<br />

![redux-middleware](https://user-images.githubusercontent.com/94499416/184522895-3fd1160c-2a98-452d-84f9-9060fe7b965c.png)

<br />

### `Middleware 구조`

미들웨어의 기본 구조를 볼 수 있는 코드를 보자.

<br />

```

const logger_middleware = store => next => action => {
  // 미들웨어 기본 구조
}

export default logger_middleware;

```

<br />

위 코드에서 리덕스 미들웨어의 구조를 볼 수 있는데 코드를 보고 이해를 못할 수 있다. 

화살표 함수를 연달아서 사용했는데 일반 function을 사용해서 풀어서 쓴다면 다음과 같은 구조다.

<br />

```

const logger_middleware = function logger_middleware(store) {
  return function(next){
    return function(store){
      // 미들웨어 기본 구조
    }
  }
}

```

<br />

미들웨어는 결국 함수를 반환하는 함수이다. 

위 코드에 함수에서 파라미터로 받아오는 파라미터는
  > store -> 리덕스 스토어 <br />
  > action -> 디스패치된 액션

next 파라미터는 함수 형태이며, store.dispatch와 비슷한 역할을 하지만 큰 차이점이 있다.

next를 호출하면 그 다음 처리해야 할 미들웨어에게 액션을 넘겨주고, 만약 그 다음 미들웨어가 없다면 리듀서에게 액션을 넘겨준다.

<br />

![next-vs-dispatch](https://user-images.githubusercontent.com/94499416/184523250-a4e891b8-7e35-40ee-8756-0b91b9ef14d0.png)

<br />

### `redux-thunk`

redux-thunk는 비동기 작업을 할 때 가장 많이 사용하는 미들웨어로

객체가 아닌 함수 형태의 액션을 디스패치 할 수 있게 해준다.

**Thunk란 ?**

Thunk는 특정 작업을 나중에 할 수 있도록 미루기 위해 함수 형태로 감싼 것을 의미한다.

아래는 주어진 파라미터에 1을 더하는 함수이다.

```

const add_one = x => {
	let temp_dom = document.getElementById("demo");    
    temp_dom.innerHTML = x + 1;
}

add_one(1);

```

이 코드를 실행하면 add_one을 호출했을때 바로 1+1이 연산된다.

만약 이 연산 작업을 나중에 하도록 미루고 싶다면 어떻게 해야할까? 아래 코드를 보자.

아래 코드는 add_one 함수의 작업을 나중에 하도록 미룬 함수다.

```

<p id="demo"></p>

<script>
function add_one (param){
	let temp_dom = document.getElementById("demo");    
    temp_dom.innerHTML = param + 1;
}

function add_one_thunk (param) {
	const thunk = function temp_func() {
    	add_one(param);
    }
    return thunk;
}

const func_run = add_one_thunk(1);
setTimeout(()=>{
	const value = func_run(); //func_run이 실행되는 시점에서 연산
    console.log(value);
}, 1000);

```

실행하면 func_run이 실행되는 시점에서 연산하기 때문에 value는 undefined가 뜨며

1초 뒤 화면에 2라는 숫자가 렌더링된다.

아래 코드는 위 function 키워드를 이용한 코드를 화살표 함수로 변경한 코드이다.

```

const add_one = (param) => {
	let temp_dom = document.getElementById("demo");    
    temp_dom.innerHTML = param + 1;
}

const add_one_thunk = param => () => add_one(param);

const func_run = add_one_thunk(1);
setTimeout(()=>{
	const value = func_run(); //func_run이 실행되는 시점에서 연산
    console.log(value);
}, 1000);

```

redux-thunk 라이브러리를 사용한다면 thunk 함수를 만드렁서 디스패치 할 수 있다.

그 후 리덕스 미들웨어가 그 함수를 전달받아 store의 dispatch와 getState를 파라미터로 넣어서 호출해준다.

다음은 redux-thunk에서 사용할 수 있는 예시 thunk이다.

```

const sample_thunk = () => (dispatch, getState) => {
  // 현재 상태를 참조,
  // 
}

```

### `Redux-thunk 웹 요청 비동기 작업을 처리하기`

본격적으로 Redux-thunk를 사용해 웹 요청 비동기 작업을 처리해보자.

구현할 기능은 input에 값을 넣으면 JSONPlaceholder에서 제공하는 값에 맞는 post 데이터를 가져오는것이다.

**1. API 호출**

```

// lib/api.js

import axios from 'axios'

export const get_post_api = id =>
  axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)

```

먼저 API를 호출하는 코드를 작성한다. 

여담이지만 {}를 넣었더니 response값으로 Promise를 반환해 당황했던적이 있다.

위 코드는 사실 아래 코드를 줄인것이다. () => {} 작성시 {}안에 return을 넣지 않아서

예상치못한 response를 반환했던것인데 나중에 다루고자 한다. 

```

// lib/api.js

import axios from 'axios'

export const get_post_api = id => {
  return axios.get(`https://jsonplaceholder.typicode.com/posts/${id}`)
}

```

**2. reducer 생성**

```

// modules/sample.js

import { handleActions } from 'redux-actions'
import * as api from '../lib/api'

// 액션 타입을 선언
// 한 요청당 시작, 성공, 실패 세개를 만들어야 한다.

const GET_POST = `sample/GET_POST`
const GET_POST_SUCCESS = `sample/GET_POST_SUCCESS`
const GET_POST_FAIL = `sample/GET_POST_FAIL`

// thunk 함수 생성
// 시작, 성공, 실패할때 다른 액션을 디스패치

export const get_post = id => async dispatch => {
  dispatch({ type: GET_POST }) // 요청 시작
  try {
    const response = await api.get_post_api(id)
    dispatch({
      type: GET_POST_SUCCESS,
      payload: response.data,
      console: console.log(response),
    }) // 요청 성공
  } catch (e) {
    dispatch({
      type: GET_POST_FAIL,
      payload: e,
      error: true,
    }) // 요청 실패
    throw e
  }
}

// 초기 상태를 선언
// 요청의 로딩 상태는 loading이라는 객체에서 관리

const initial_state = {
  loading: {
    GET_POST: false,
  },
  post: null,
}

const sample = handleActions(
  {
    [GET_POST]: state => ({
      ...state,
      loading: {
        ...state.loading,
        GET_POST: true,
      },
    }),
    [GET_POST_SUCCESS]: (state, action) => ({
      ...state,
      loading: {
        ...state.loading,
        GET_POST: false,
      },
      post: action.payload,
    }),
    [GET_POST_FAIL]: state => ({
      ...state,
      loading: {
        ...state.loading,
        GET_POST: false,
      },
    }),
  },
  initial_state
)

export default sample

```

**3. root reducer에 포함**

```

import { combineReducers } from 'redux'
import sample from './sample'

const root_reducer = combineReducers({
  sample,
})

export default root_reducer

```

**4. 컴포넌트 제작**

```

import react from 'react'
import { connect } from 'react-redux'
import Sample from './Sample'
import { get_post } from '../modules/sample'

const SampleContainer = ({ get_post, post, loading_post }) => {
  const change_id = param => {
    const { value } = param.target

    get_post(value)
  }

  return (
    <>
      <input
        type="number"
        onChange={e => {
          change_id(e)
        }}
      />
      <Sample post={post} loading_post={loading_post} />
    </>
  )
}

export default connect(
  ({ sample }) => ({
    post: sample.post,
    loading_post: sample.loading.GET_POST,
  }),
  { get_post }
)(SampleContainer)


```
