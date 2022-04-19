# ReactHooks

  * 보통 리덕스를 사용하는것이 편하긴하다.
  * 하지만 상위 -> 하위 또는 하위 -> 상위 컴포넌트로 데이터를 전달하는 상황이 많지 않을 때를 위해 적어본다.

  ## 상위 컴포넌트에서 하위 컴포넌트로 데이터 전달

  ![image](https://user-images.githubusercontent.com/94499416/163962665-c7251424-b9c3-49b2-b60c-d579e3fa6462.png)

  ParentComponent.js
  
  ![image](https://user-images.githubusercontent.com/94499416/163962119-fb57708d-5148-4680-a6b8-dd405f3a1d75.png)
  
  ChildComponent.js
  
  ![image](https://user-images.githubusercontent.com/94499416/163962467-e4c90f47-f781-438b-bd1e-55dd780b58e7.png)
  
  * props를 사용합니다. (properties를 줄인말로 컴포넌트의 속성을 뜻합니다).
  * 상위 컴포넌트에서 설정할 수 있으며 상위 -> 하위로만 데이터를 줄 수 있다.
  * 하위 컴포넌트 -> 상위 컴포넌트를 props를 통해 전달이 불가능하다.

  ## 하위 컴포넌트에서 상위 컴포넌트로 데이터 전달
  
  ![image](https://user-images.githubusercontent.com/94499416/163966364-fc6cda85-31c3-4c39-99bd-19db7fd79c8b.png)
  
  ParentComponent.js
  
  ![image](https://user-images.githubusercontent.com/94499416/163966758-c2f45e7f-d6cb-488f-bc54-17ed23d01012.png)
  
  ChildComponent.js
  
  ![image](https://user-images.githubusercontent.com/94499416/163967271-eb0639d6-6318-418e-b9f6-67c845568bd3.png)
  
  * 자식은 props를 사용해서 상위 컴포넌트에 데이터를 줄 수 없다.
  * 따라서 상위 컴포넌트가 함수를 넣어 props로 하위 컴포넌트에 넘겨주면, 하위 컴포넌트에서는 데이터를 파라미터로 넣어 호출하는 방식을 사용한다.
  * 상위 컴포넌트가 props로 함수를 넣어주면 하위 컴포넌트가 그 함수를 이용해 값을 건네주는 방식인것이다.
  
  
