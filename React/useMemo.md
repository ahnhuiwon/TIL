## useMemo

### `useMemo란 무엇인가?`

렌더링 최적화를 위해 사용하는 react hook.

### `렌더링 최적화`

react는 상위 컴포넌트로 받는 state와 props가 변동될 경우 리렌더링된다.

이 방식은 문제가 존재하는데

> state와 props가 여러개 존재한다면? <br />
> state 하나만 변경을 했는데 다른 state들도 재계산된다면 이 상황을 좋은 리렌더링이라 할 수 있는가?

useMemo와 useCallback은 이 문제점을 생각한 개발자들을 위해 구현됬다고 보면 된다.

### `useMemo 사용법`

useMemo를 사용하기 위한 간단한 코드를 짜보자.

파일 구성은 state를 변경하는 App.js와 console.log와 props값을 출력하는 ShowProps.js로 구성되어 있다.

```
// APP.js

import { useState } from 'react';
import ShowProps from './component/ShowProps';

function App() {

  const [number, set_number] = useState(0);
  const [name, set_name] = useState('');

  const up_count = () => {
    set_number(number + 1);
  }

  const down_count = () => {
    set_number(number - 1);
  }

  const name_change = (e) => {
    set_name(e.target.value);
  }

  return (
    <div className="App">
      <div>
        <button onClick={up_count}>+</button>
        <button onClick={down_count}>-</button>
        <br />
        <input type="text" placeholder='name' onChange={name_change} />
      </div>
      <ShowProps number={number} name={name}/>
    </div>
  );
}

export default App;
```

```
// ShowProps.js

import React, { useState } from "react";

const ShowProps = ({number, name}) => {

    const get_number = (number) => {
        console.log('숫자가 변동됨');
        return number;
    }

    const get_name = (name) => {
        console.log('이름이 변동됨');
        return name;
    }

    const show_number = get_number(number);
    const show_name = get_name(name);

    return (
        <>
            <p>{show_number}</p>
            <p>{show_name}</p>
        </>
    )
}

export default ShowProps;
```

한번 +, -버튼을 누르거나 input창에 텍스트를 입력해보면

아래와 같은 console.log를 확인할 수 있다.

> 숫자가 변동됨
> 이름이 변동됨

무엇이 문제점인지 바로 알 수 있다. 

사용자는 숫자만 변경했음에도 글자가 변동되었다는 console.log가 출력된다.

**변경하고자 하는 상태값이 아닌 함수도 실행이 되는 비효율적이고 낭비**라고 생각할 수 있다.

이럴 때 useMemo를 사용하는것이다.

ShowProps내에서 useMemo를 써보자.

> const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

우리가 memozation할 값은 show_number와 show_name이므로 이 둘을 적용시켜보자.

```
// ShowProps.js

import React, { useMemo } from "react";

const ShowProps = ({number, name}) => {

    const get_number = (number) => {
        console.log('숫자가 변동됨');
        return number;
    }

    const get_name = (name) => {
        console.log('이름이 변동됨');
        return name;
    }

    const show_number = useMemo(()=>get_number(number),[number]);
    const show_name = useMemo(()=>get_name(name),[name]);


    return (
        <>
            <p>{show_number}</p>
            <p>{show_name}</p>
        </>
    )
}

export default ShowProps;
```

적용을 시키고 버튼을 눌러보거나 input창에 텍스트를 넣어보자.

그렇다면 전과 달리
> number 상태값이 변하면 '숫자가 변동됨'
> name 상태값이 변하면 '이름이 변동됨'

console.log를 확인할 수 있다.

하지만 위 코드에서는 useMemo의 사용으로 성능의 큰 영향을 주지 않는다.

매우 간단한 계산으로만 구성되었기 때문인데

**memoization 되는 값의 재계산 로직이 복잡한 로직일 경우 useMemo를 사용하는것**이 매우 좋고

이런 경우에는 성능상 큰 장점으로 작용한다.

