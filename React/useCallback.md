## useCallback

### `useCallback이란 무엇인가?`

useMemo처럼 렌더링 최적화를 위해 사용하는 react hook.

### `useMemo 사용법`

useCallback은 useMemo와 매우 유사하다는 특이점이 있다.

파일 구성은 state를 변경하는 App.js와 console.log와 props값을 출력하는 ShowCallback.js로 구성되어 있다.

```
// APP.js
import './App.css';
import { useState } from 'react';
import ShowCallback from './component/ShowCallback';

function App() {

  const [number, set_number] = useState(1);
  const [theme, set_theme] = useState(false)  

  const get_number = () => {
    return [number, number + 1, number + 2];
  }

  return (
    <div className="App">
      <input type="number" value={number} onChange={(e)=>{ set_number(Number(e.target.value)) }}/>
      <button onClick={()=>{ set_theme(!theme) }}>테마 변경</button>
      <ShowCallback get_number={get_number} theme={theme}/>
    </div>
  );
}

export default App;
```

```
// ShowCallback.js
import React, { useEffect, useState } from "react";

const ShowCallback = ({get_number}) => {

    const [item, set_item] = useState([]);
    const [my_theme, set_my_theme] = useState('');

    useEffect(()=>{
        set_item(get_number());
        console.log('숫자 변동 감지')
    }, [get_number]);

    return item.map((data, index) => <div key={index}>{data}</div>)
}

export default ShowCallback;
```

input의 값을 변경하면 출력되는 숫자 데이터들이 변경되고 테마 변경이라는 버튼을 누르면

디자인 요소가 변경되는 간단한 코드이다.

숫자를 변경한다면 console.log를 확인할 수 있다.

> 숫자가 변동 감지 <br/>

라는 console.log를 확인할 수 있다.

하지만 사용자는 테마 변경 버튼을 클릭했음에도 숫자 변동 감지라는 console.log가 출력된다.

**테마만 변경하는 함수를 실행했을 뿐인데 get_number라는 함수도 실행되는 것이다.**

>상위 컴포넌트에서 받은 get_number라는 함수로 구성된 props는 <br />
>상위 컴포넌트가 리렌더링되면서 변경된 props로 인식되기 떄문에 발생하는 현상이다. <br />
>그렇기 때문에 부모 컴포넌트의 number 값이 새로 설정되어 해당 함수가 계속 반복된다.

<br />

number에 대한 의존성을 가지는 useCallback으로 get_number 함수를 매핑시켜주자.

React 공식 홈페이지에서 제공하는 useCallback 코드이다.

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

<br />

```
//App.js

import './App.css';
import { useCallback, useState } from 'react';
import ShowCallback from './component/ShowCallback';

function App() {

  const [number, set_number] = useState(1);
  const [theme, set_theme] = useState(false)  

  const get_number = useCallback(() => {
    return [number, number + 1, number + 2];
  }, [number])

  const my_theme = {
    backgroundColor: theme ? "#333" : "#fff",
    color: theme ? "#fff" : "#333",
  }

  return (
    <div className="App" style={my_theme}>
      <input type="number" value={number} onChange={(e)=>{ set_number(Number(e.target.value)) }}/>
      <button onClick={()=>{ set_theme(!theme) }}>테마 변경</button>
      <ShowCallback get_number={get_number}/>
    </div>
  );
}

export default App;
```

테마 변경을 클릭하면 더 이상 console.log가 뜨지 않는걸 확인할 수 있다.

### useMemo와 useCallback의 차이점

useMemo와 useCallback은 유사한 점이 많지만 분명한 차이점이 있다.

이 두가지 hook이 어떻게 다르고 써야하는지를 알아보자.

```
// useCallback을 사용한 코드
const get_number = useCallback((param) => {
    return [
      number + param, 
      number + param + 1, 
      number + param + 2];
  }, [number])

// useMemo를 사용한 코드
const get_number = useMemo(() => param => {
  return [
    number + param,
    number + param + 2,
    number + param + 3
  ]
}, [number])
```

위 두 코드의 차이점을 확인해보자.

useCallback(함수, 의존성 배열)은 useMemo(()=> 함수, 의존성 배열)과 같다.

그렇다면 이렇게 선언을 해줘야하는 이유는 무엇인가?

>useMemo는 함수를 반환하지 않고, 함수의 값만 메모리에 저장해 동일한 계산의 반복 수행을 제거한다. <br />
>useCallback은 함수 자체를 메모리에 저장해 동일한 계산의 반복 수행을 제거하는데 <br />
>이 차이점이 useMemo와 useCallback의 핵심적인 차이인 것이다.
  
결론은 하위 컴포넌트에서 특정한 props값의 변화를 최적화 시키고 싶다면 useMemo를

상위 컴포넌트에서 계산량이 많은 props 함수를 자식 컴포넌트로 넘기고 싶을떄는 useCallback을 쓰는것이 좋다.
