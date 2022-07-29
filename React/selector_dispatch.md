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

useSelector는 문제가 있는데 이 문제를 알아보기 위해

코드를 작성해보자.
