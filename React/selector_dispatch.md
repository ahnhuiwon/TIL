## useSelector

### `useSelector란 무엇인가?`

리덕스의 상태값을 조회하기 위한 hook 함수.

이전에 언급했던 connect를 통해 상태값을 조회하는 것보다 훨씬 간결하고

가독성이 상승되는 장점이 있는 함수이다.

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

위 코드를 참고해서 **useSelector**를 사용하는 간단한 예제를 만들어보자.
