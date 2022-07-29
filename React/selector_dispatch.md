## useSelector와 useDispatch

### `useSelector란 무엇인가?`

리덕스의 상태값을 조회하기 위한 hook 함수.

이전에 언급했던 connect를 통해 상태값을 조회하는 것보다 훨씬 간결하고

가독성이 상승되는 장점이 있는 함수이다.

### `useDispatch란 무엇인가?`

redux의 액션 함수를 실행시켜

redux-store에 변경된 state값을 저장하기 위한 hooks

### `useSelector 사용법`

```

// rootReducer에 있는 state값 가져오기

const redux_data = useSelector(state => state.my_data)

// rootReducer에 있는 여러 state값 가져오기

const { my_data, temp_data }  = useSelector(state => ({
     my_data : state.my_data,
     temp_data: state.temp_data,
}));

```

### `useDispatch 사용법`

```

const disaptch = useDispatch();

dispatch(set_data(value));

```

위 코드를 참고해서 **useSelector와 useDispatch**를 사용하는 간단한 예제를 만들어보자.

```

//App.js

import './App.css';
import { useSelector } from 'react-redux';
import ShowRedux from './component/ShowRedux';

function App() {

  const redux_data = useSelector(state => state.my_data);

  return (
    <div className="App">
      <h1>상위 컴포넌트</h1>
      <p>{redux_data}</p>
      <ShowRedux />
    </div>
  );
}

export default App;

```

```

//ShowRedux.js

import React from "react";
import { useDispatch } from "react-redux/es/exports";
import { set_data } from "../redux/action";

const ShowRedux = () => {

    const dispatch = useDispatch();

    const change_state = (e) => {
        const { value } = e.target;

        dispatch(set_data(value));
    }

    return(
        <>
            <h1>하위 컴포넌트</h1>
            <input onChange={(e)=>{change_state(e)}}/>
        </>
    )
}

export default ShowRedux;

```

위 코드는 하위 컴포넌트인 ShowRedux에서 input은 onChange 이벤트를 감지하면

dispatch를 통해 리덕스 상태값을 input value의 값으로 변경하고

상위 컴포넌트인 App에서 useSelector를 이용해 변경된 리덕스 상태값을 가져와 렌더링 시켜주는 아주 간단한 코드이다.

### `useSelector 최적화`

useSelector는 여러 state값을 가져올떄 

문제가 있는데 이 문제를 알아보기 위해

코드를 작성해보자.

```

// Number.jsx

import React from "react";
import { useDispatch, useSelector } from "react-redux";
import { set_number } from "../redux/action";

const Number = () => {
     
    // 변경된 부분
    const { my_number, my_data} = useSelector((state) => ({
        my_number : state.my_number,
        my_data : state.my_data
    }));
    
    const dispatch = useDispatch();

    const up_count = () => {
        dispatch(set_number(my_number + 1));
    }

    return(
        <>
            <p>{my_number}</p>
            <button onClick={()=>{ up_count() }}>+</button>
        </>
    )
}

export default Number;

```

```

// Input.jsx

import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { set_data } from "../redux/action";

const Input = () => {
    
    // 변경된 부분
    const { my_number, my_data} = useSelector((state) => ({
        my_number : state.my_number,
        my_data : state.my_data
    }));

    const dispatch = useDispatch();

    const change_state = (e) => {
        const { value } = e.target;
        dispatch(set_data(value));
    }


    return(
        <>
            <p>{my_data}</p>
            <input type="text" onChange={(e)=>{ change_state(e) }}/>
        </>
    )
}

export default Input;

```

코드를 여러 reducer의 값을 한번에 가져오는 코드로 변경했다.

react-dev-tool을 사용해 최적화 여부를 확인해보자.

![캡처](https://user-images.githubusercontent.com/94499416/181689757-8d4dbeb6-316d-4f14-b057-9f861209e74b.PNG)

하늘색으로 표시된 부분이 컴포넌트가 렌더링 되었다는 뜻이다.

두개의 컴포넌트는 서로 다른 reducer 값을 조회 안하지만 그럼에도 전부 리렌더링이 되고 있다.

#### 다시 렌더링이 되는 이유는?

```

const { my_number, my_data} = useSelector((state) => ({
        my_number : state.my_number,
        my_data : state.my_data
    }));

```

코드를 보자.

my_number와 my_data는 조회할때 다시 객체를 생성하는 방식으로 선언되었다.

이러한 이유로 react에서는 상태가 바뀌는 것의 여부를 파악할 수 없어 다시 렌더링을 해버린다.

#### 첫번째 해결방법 - 독립선언

위에 코드에서 선언한것처럼 객체 방식이 아닌 각각의 값을 독립적으로 선언하면

이에 대한 상태 변경 여부를 파악할 수 있어 최적화 할 수 있다.

```

const my_data = useSelector(state => state.my_data);
const my_number = useSelector(state => state.my_number);

```



