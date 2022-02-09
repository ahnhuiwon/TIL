# ReactHooks
  ## useReducer

  * useReducer란?
    * useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을때 사용하는 Hook.
    * useReducer는 Redux의 기반으로 사용된다.

  
  ## useReducer 아키텍처.

  ![useReducer](https://user-images.githubusercontent.com/94499416/153228441-9ff78f6d-eeee-4cda-8a6c-2eb7ae3a1413.svg)
  
    * 이벤트 핸들러의 결과, 가져오는 요청을 완료하면 dispath 함수 매개변수 자리에 action.object를 담아 호출한다.
    * React는 action object와 현재 state 값을 ruducer 함수로 리디렉션한다.
    * Reducer 함수는 action object를 사용해 state값 업데이트를 수행하여 새로운 상태를 반환한다.
    * 이전 상태값과 다른지 비교, 상태값이 업데이트되면 리 렌더링하고 useReducer()의 새 상태값을 반환한다.




  # useReducer() 사용
  
  ![스크린샷, 2022-02-10 00-35-30](https://user-images.githubusercontent.com/94499416/153234737-6cde1ea1-41bb-402b-b4f6-2a01f19645c2.png)
    
    * useReducer(reducer, initailState) Hook은 Reducer 함수와 초기 state 값이란 인수를 허용한다.
    * 그 다음 useReducer Hook은 현재 state값과 dispatch() 함수로 구성된 배열을 반환한다.

  ## A. 초기 상태 설정
  
  ![스크린샷, 2022-02-10 00-50-01](https://user-images.githubusercontent.com/94499416/153237642-6437e566-51e0-42bf-a9a3-62c609291872.png)
  
    * 초기 상태는 state가 초기화 되는 값이다.
    * 위 예시는 카운터 상태가 초기화되는 설정입니다. 

  ## B. Action 객체
  
  ![스크린샷, 2022-02-10 01-05-45](https://user-images.githubusercontent.com/94499416/153240814-452d3cd3-7baa-4974-a169-0a7d2349cfff.png)
  
    * action 객체는 상태를 업데이트하는 방법을 설명하는 객체이다.
    * 일반적으로 action 객체에는 type이라는 속성이 있다. 
    * reducer가 수행해야하는 상태 업데이트의 종류를 설명하는 '문자열'이다.

  ## C. dispatch() 함수
  
  ![스크린샷, 2022-02-10 00-35-30](https://user-images.githubusercontent.com/94499416/153234737-6cde1ea1-41bb-402b-b4f6-2a01f19645c2.png)
  
    * dispatch는 action 객체를 보내는 특별한 함수입니다.
    * dispatch 함수는 useReducer() Hook에 의해 생성된다.
    * state를 업데이트 하고 싶을때마다 적절한 action 객체를 dispatch()의 인자로 넣어서 호출한다.
    * -> dispatch(action_object)
    
  ## D. Reducer() 함수
  
  ![스크린샷, 2022-02-10 01-20-19](https://user-images.githubusercontent.com/94499416/153243538-a0e1cbdd-6150-4532-bb6b-31a80e26cb46.png)
  
    * reducer()는 현재 state와 action 객체, 2개의 매개변수를 받는 순수 함수이다.
    * action 객체에 따라 reducer 함수는 변경할 수 없는 방식으로, 상태를 업데이트하고 새 상태를 반환하는 방법을 사용한다.
    * 위 예제는 변수의 현재 상태를 직접 수정하지 않고 state에 새 상태 객체를 newState에 반환한다.
    * React는 새로운 상태와 현재 상태의 차이를 확인해 상태가 업데이트 되었는지 확인하기 때문에 
