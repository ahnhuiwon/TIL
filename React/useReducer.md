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
    * React는 새로운 상태와 현재 상태의 차이를 확인해 상태가 업데이트 되었는지 확인하기 때문에 직접 변경하지 않는다.
    
  ## useReducer로 스톱워치 구현하기
    * 아래 예제는 스톱워치를 구현하는 코드이다.
    * 스톱워치에는 시작과 중지, 초기화 기능을 가진 버튼 3개와 초를 나타내는 숫자로 구성된다.
    
  ![initial_state](https://user-images.githubusercontent.com/94499416/153442761-710e5291-3d3b-478c-87de-554928b102d2.png)
    
    * 스톱워치의 실행 여부를 나타내는 is_play 속성과 경과된 초를 나타내는 second라는 속성의 초기값으로 설정해준다.
    * 초기 상태는 스톱워치가 비활성화 상태로 0초부터 시작함을 나타낸다. 
    
  ![스크린샷, 2022-02-11 01-04-04](https://user-images.githubusercontent.com/94499416/153447129-393820f1-55ca-4530-94b6-bd59eadc3ed5.png)
     
    * 스톱워치에 필요한 4가지 action 객체를 만든다. 스톱워치를 실행, 중지, 초기화, 매 초마다 카운터를 세는 action이다. 
    
  ![스크린샷, 2022-02-11 01-08-04](https://user-images.githubusercontent.com/94499416/153447885-420dcfb3-4a75-43ab-955e-5573da3b4f83.png)
  
    * reducer() 함수를 이용하여 action 객체로 상태를 업데이트하는 코드를 작성한다.
    
  ![스크린샷, 2022-02-11 01-10-24](https://user-images.githubusercontent.com/94499416/153448509-43d1abe7-518b-4872-b90d-c85f6d65d0d2.png)
  
    * 스톱워치의 시작, 중지, 리셋 버튼의 클릭 이벤트 핸들러들은 각각의 dispatch() 기능을 통해 action 객체를 전달한다.
    * useEffect() 콜백 내에서 state.is_play가 true인 경우 setInterval() 함수를 통해 매초마다 'PLAY' action 객체를 전달한다.
    * 함수가 상태를 업데이트 할때마다 reducer() 구성 요소는 다시 렌더링되고 새 상태를 받는다.
    
  ![stopwatch](https://user-images.githubusercontent.com/94499416/153449819-2754d557-fc25-40fb-88f0-5833f187f22a.gif)
  
    * useReducer()를 활용해 스톱워치를 구현한 모습이다.
