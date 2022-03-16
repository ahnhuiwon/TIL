# ReactHooks
  ## useRef

  * useRef란?
    * DOM을 꼭 직접적으로 건드려야 할 떄 사용한다.
    * useRef는 리렌더링 하지 않고 컴포넌트의 속성을 조회&수정한다.
    * 선언적으로 해결할 수 있는 문제는 ref 사용을 자제한다.


  # 컴포넌트에 focus를 위치시킬 필요가 있는 경우.
    
 ![reset_ref](https://user-images.githubusercontent.com/94499416/158188675-322167cc-4a07-45de-bccb-6a2e2b23e93c.gif)
    
    * 위 이미지는 여러 input들과 초기화 버튼이 존재하고 버튼을 누른다면 모든 state값이 초기화되고 첫번째 input으로 focus되는 이미지이다.
    * 여러 input에 값을 여러개 입력하고 유저가 초기화 버튼을 누른다면 첫번째 input으로 이동해야하는 경우 필요하다.
    
  ## A. 초기값 설정
  
  ![ref](https://user-images.githubusercontent.com/94499416/158192644-d6f8f507-7ff8-4985-96eb-74ac84b739ac.png)
  
    * state 초기값과 ref 객체를 선언해준다.
    
  ## B. DOM 속성에 Ref 설정해준다.
  
  ![스크린샷, 2022-03-14 23-28-20](https://user-images.githubusercontent.com/94499416/158193333-5edf66d9-3e1b-4d97-9fac-56224e106a27.png)

    * 선택하고 싶은 DOM 객체에 속성으로 Ref 값을 설정한다.
    * 첫번째 input에 ref={name_ref}로 Ref 값을 설정한 코드이다.
    
  ## C. DOM API focus() 호출
  
  ![스크린샷, 2022-03-14 23-34-26](https://user-images.githubusercontent.com/94499416/158194515-c2f5f7a2-2c7d-40c1-85ca-6bb9ee895bcc.png)
  
    * ...(스프레드 문법)으로 복사한 state값을 object_reset이란 변수에 할당.
    * Object.keys()와 map() 함수로 객체의 속성 수 만큼 반복해서 초기화 시킨다.
    * 초기화 시킨 값을 set_user_state를 이용해 user_state에 할당.
    * Ref의 current값으로 우리가 선택하고자 하는 dom을 가르키며 DOM API focus()를 호출, 포커싱을 해준다.
    
 # Ref로 setInterval과 setTimeout 함수 초기화(clear) 하기
 
 ![Peek 2022-03-15 01-09](https://user-images.githubusercontent.com/94499416/158213682-07833bfb-14dd-4ed8-a88e-5c25d2aaa2ef.gif)
 
    * 위 이미지는 컴포넌트가 mount 됬을때 count가 증가하고 unmount시 카운트가 초기화되는 이미지이다.
    * setInterval과 setTimeout 같은 함수는 clear해주지 않으면 메모리를 많이 소모한다.
    * 따라서 함수를 구현하고 unmount 혹은 특정 상황시 clear해줄 필요가 있다.
    
 ## A. 초기값 설정
 
 ![스크린샷, 2022-03-15 01-12-56](https://user-images.githubusercontent.com/94499416/158214522-878e9800-ad74-4c0d-8dbf-079a5c37c156.png)
 
    * state 초기값과 ref 객체를 선언해준다.
 
 ## B. setInterval과 clearInterval 설정
 
 ![스크린샷, 2022-03-15 01-15-34](https://user-images.githubusercontent.com/94499416/158214876-4754e897-1130-4bff-a239-62d8989ea909.png)
 
    * Ref 객체에 setInterval 함수를 넣어준다.
    * 컴포넌트가 언마운트될때 clearInterval 함수를 통해 setInterval 함수가 들어있는 Ref 객체를 초기화시킨다. 
    
 # useRef로 컴포넌트 안의 변수를 관리할 경우
 
    * Dom 선택 용도 외에도, 컴포넌트 안에서 조회 및 수정 가능한 변수를 관리하는 용도가 있다.
    * useRef로 변수를 관리한다면, 변수가 업데이트 되어도 컴포넌트가 리렌더링 되지 않는다.
    * 리렌더링이 필요 없는 변수라면 useRef로 관리해주는게 효율적이다.
    * setInterval을 통해 만들어진 id, scroll 위치, 배열에 새 항목을 추가할 때 필요한 고유 key등에 쓰인다.
    
 ![Peek 2022-03-17 00-31](https://user-images.githubusercontent.com/94499416/158627619-4d4965fb-7f03-4567-8182-ae30efa5b386.gif)
 
    * 위 이미지는 배열에 새로운 항목을 추가하는 이미지이다.
    
## A. 초기값 설정
    
 ![스크린샷, 2022-03-17 00-34-10](https://user-images.githubusercontent.com/94499416/158628279-f16463f3-d64b-4675-bb29-c06736e5fb9d.png)
 
    * state 초기값과 ref 객체를 선언해준다.
    * 배열의 고유 값(id) 변수로 next_id = useRef(1)을 선언해준다.
    
 ![스크린샷, 2022-03-17 00-41-09](https://user-images.githubusercontent.com/94499416/158629763-d046102c-3d01-40b2-ae9f-83305ccace17.png)
 
    * input value값을 가져와 상태값을 변경하는 코드이다.
    * ...(스프레드 연산자)과 [e.target.name](속성 접근자) 문법을 사용해 상태값을 변경한다.
    
 ![스크린샷, 2022-03-17 00-45-07](https://user-images.githubusercontent.com/94499416/158630547-f206a0d6-1366-411b-9a3b-e42b6bc1b157.png)
 
    * useRef() 파라미터 값을 넣어주면 해당 값이 변수의 current 값이 된다.
    * 프레드 연산자를 사용해 불변성을 지키면서 배열에 새 항목을 추가한다.
    * next_id 변수를 수정, 조회를 한다면 current 값을 수정하거나 조회한다.
    * 배열에 새 항목이 생긴다면 next_id 변수에 1씩 더하여 업데이트 한다.
