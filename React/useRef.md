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
  
    * ...(스프레드 문법)으로 복사한 state값을 object_reset이란 변수에 할당한 뒤 Object.keys()와 map() 함수로 객체의 속성 수 만큼 반복해서 초기화 시킨다.
    * 초기화 시킨 값을 user_state에 할당 시킨 뒤 Ref의 current값으로 우리가 선택하고자 하는 dom을 가르키며 DOM API focus()를 호출, 포커싱을 해준다.

