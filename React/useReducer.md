# ReactHooks
  ## useReducer

  * useReducer란?
    * useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을때 사용하는 Hook.
    * useReducer는 Redux의 기반으로 사용된다.


  ![useReducer](https://user-images.githubusercontent.com/94499416/153228441-9ff78f6d-eeee-4cda-8a6c-2eb7ae3a1413.svg)
  
  * useReducer 아키텍처.
  
  * 이벤트 핸들러의 결과, 가져오는 요청을 완료하면 dispath 함수 매개변수 자리에 action.object를 담아 호출.
  * React는 action object와 현재 state 값을 ruducer 함수로 리디렉션한다.
  * Reducer 함수는 action object를 사용해 state값 업데이트를 수행하여 새로운 상태를 반환.
  * 이전 상태값과 다른지 비교, 상태값이 업데이트되면 리 렌더링하고 useReducer()의 새 상태값을 반환.
