# Redux
  ## Redux 설명

  * Redux란?
    * 자바스크립트를 위한 예측 가능한 state container이다.
    * 리액트 뿐만 아니라 Angular, jQuery, vanilla JavaScript 등 다양한 프레임워크에서도 작동한다.
    * 엄연히 말하면 리액트만을 위한 라이브러리는 아닌셈이다.
  
  
  * React에서 리덕스가 필요한 이유?
    * 리액트로 프로젝트를 진행하면 Component는 localState를 소유하지만 어플리케이션은 global state를 가지게 된다.
    * 리액트로만 프로젝트를 진행하게 될 경우 어플리케이션은 local state, global state를 관리하기 어렵다.

  
    ## local state 전달의 어려움
  
    ![cart_flow_chart](https://user-images.githubusercontent.com/94499416/161573452-e84ea092-1d49-47e0-804d-555563debb98.png)
    
      * 프로젝트의 규모가 커지고 component의 수가 늘어난다면 props data에 필요없는 흐름이 생긴다.
      * 하위 컴포넌트에서 props 데이터가 전달이 안 될 경우, 중간에 존재하는 component에서 일일이 문제점을 찾아야한다.
      * 허나 리덕스를 사용한다면 하나의 store를 통해 global state 및 모든 state를 저장 유지 할 수 있다.
