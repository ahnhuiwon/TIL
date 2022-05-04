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



    ## Flux 구조
  
    ![flux_img](https://user-images.githubusercontent.com/94499416/161574600-22c71838-f270-497c-967c-0b4db16e9f11.png)
    
      * 페이스북에서 만든 클라이언트 사이드 웹 어플리케이션을 구축할때 사용하는 앱구조, 디자인 패턴이다.
      * MVC구조의 단점을 보완할 목적으로 개발되었으며 대규모 프로젝트에서 너무 복잡해지는 MVC 구조의 단점을 보완하는 단방향 데이터 흐름의 구조이다.
      * Flux를 소개하는 이유는 리덕스가 Flux의 구현체라고 할 수도 있기 때문이다.



    ## Flux VS Redux
    
    ![flux_redux](https://user-images.githubusercontent.com/94499416/161575187-409c0f9a-4c95-4b67-b037-d01657f347ff.png)
    
      * Flux와 달리 리덕스는 dispatcher라는 개념이 존재하지 않는다.
      * 다수의 store도 존재하지 않고 리덕스는 하나의 root에 단 한가지 store만이 존재한다.
      * 리덕스는 순수함수에 의존하는데, 이 순수함수는 추가적인 개체 없이 조합하기 쉽다.
      * 결론적으로 Flux, 리덕스 두 구조는 모두 닮았으며 리덕스는 Flux 패턴을 좀 더 쉽고 정돈된 형태로 쓸 수 있게 해주는 라이브러리라고 볼 수 있다. 


    ## Redux 모듈 설치

    * Redux 모듈 설치
      * yarn add redux react-redux를 입력해 두 모듈을 설치한다.
      * npm install redux, npm install react-redux를 입력해 두 모듈을 설치한다.
    
    * Reduxt-devtools 
      * npm install -D redux-devtools를 입력해 개발자 툴킷을 설치한다.
      * 스토어 값이 바뀌는 상태를 쉽게 확인 할 수 있다.
      * 개발단계에서만 사용한다면 옵션 -D를 붙여주면된다.


   # Redux 기본개념
  
   ## A. store(스토어)
    
   ![스크린샷, 2022-04-05 00-43-23](https://user-images.githubusercontent.com/94499416/161581474-79370a66-1698-45c8-9412-0cc12dd66524.png)
    
      * action과 reducer를 저장하는 어플리케이션에 있는 단 하나의 객체이다.
      * 위와 같은 코드로 store를 생성, 첫번째 인자로 reducer를 받는다. 여기서는 rootReducer이다.
      * 위 코드에서 createStore의 두번째 인자는 개발자 도구를 적용하기 위한 코드이다.
      
      
   ## B. Reducers(리듀서)
    
   ![스크린샷, 2022-05-04 10-04-57](https://user-images.githubusercontent.com/94499416/166610658-a44181ea-881c-4980-b915-63b65461cd08.png) <br>
   ![스크린샷, 2022-05-04 10-05-19](https://user-images.githubusercontent.com/94499416/166610661-ca16010d-9e2b-4224-88dc-4e559aa834e7.png)
    
      * action을 통해 어떠한 행동을 정의했다면, 그 결과 어플리케이션의 상태가 어떻게 바뀌는지 특정하는 함수이다.
      * state와 action이 들어가며, state에 초기값을 지정해줄 수 있다.
      * 위 코드에는 action을 setState처럼 쓸 수 있는 SET_STRING을 만들어 주었다.
      * 기존 state 객체를 복사, string_data라는 key와 action에서 받아오는 payload를 value로 state를 변경하는 역할을 한다.
     
   ## C. action(액션)
    
   ![스크린샷, 2022-04-05 00-35-50](https://user-images.githubusercontent.com/94499416/161580021-fa204e6c-3178-4042-b4eb-7f5663553896.png)
    
      * 어플리케이션의 스토어, 즉 저장소로 data를 보내는 방법이다.
      * view에서 정의되있는 액션을 호출하면 액션 생성자는 어플리케이션의 state를 변경해준다.
      * 위 코드에서 set_string은 string이라는 새로 변경해줄 데이터를 받아서 reducer에 전달해주는 액션이다.
      
      
   # Store에 있는 state 사용하기
   
   ## A. index.js
   
   ![스크린샷, 2022-04-05 00-52-34](https://user-images.githubusercontent.com/94499416/161583317-38b926f7-0cd6-470c-9ef6-8c9c7d16afbc.png)
   
      * index.js 파일에서 Provider를 통해 store를 하위 컴포넌트들에게 전달해준다.
   
   
   ## B. store의 state값을 변경하는 컴포넌트
   
   ![스크린샷, 2022-04-05 00-56-14](https://user-images.githubusercontent.com/94499416/161583906-5efad1b9-733a-46a3-a8ad-3d96e522d5ad.png)
   
      * 컴포넌트에서 액션을 통해 store에 있는 state를 변경하기 위해선 connect를 사용해 mapDispatchToProps와 해당 컴포넌트(Order.js)를 연결해야한다.
      * connect의 첫번째 인자는 아래에서 후술할 mapStateToProps인데 state를 변경만하기 떄문에 null값으로 둔다.
      
      
   ## C. store의 state값을 사용하는 컴포넌트
   
   ![스크린샷, 2022-04-05 01-01-55](https://user-images.githubusercontent.com/94499416/161584947-6152f090-23de-4e0a-91e4-73bc3442ea5f.png)
   
      * 컴포넌트에서 store에 있는 state(string)를 사용하려면 connect 함수를 이용해 mapStateToProps와 Header 페이지를 연결한다.
      * 연결해주고나면 <Header> 컴포넌트 안에 존재하는 <Example> 컴포넌트에서 props.strings로 state 값을 사용할 수 있다.
      
   
   ## D. 마무리
   
   ![스크린샷, 2022-04-05 01-08-44](https://user-images.githubusercontent.com/94499416/161586240-2ade72e6-3e37-45bb-8141-0e435b47367c.png)
   
   * 어플리케이션 구조는 다음과 같다.
   * 만약 리덕스를 사용하지 않았다면 props는 Order.js => App.js => Header.js => Example.js로 흘러가는 귀찮은 코드를 짰을거다.
   * 아래 이미지는 Order.js 컴포넌트에서 redux를 사용해 state값 변경시 Example.js에서도 변경된 state값이 출력이 되는 이미지이다.
   
   ![Peek 2022-04-05 01-15](https://user-images.githubusercontent.com/94499416/161587309-a04c2c3a-36dc-4dcb-ba69-4d25f8d0e040.gif)
      
      
      
