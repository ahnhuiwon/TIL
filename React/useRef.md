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

