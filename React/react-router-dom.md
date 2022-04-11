# React-router-dom
  ## React-router-dom v6
    
     React-router-dom v6를 사용한다면 React 16.8 이상에 버전을 사용해야한다.
     그래도 이전 버전에 React-router v5는 React 15와 호환된다.
    
    
  # 설치 방법
    $ yarn add react-router-dom --save
    $ npm install react-router-dom --save
    $ yarn add history@5 react-router-dom@6 (기존 react-router-dom v5를 v6로 업데이트)
    
    설치 후 package.json에서 버전을 확인해본다.
    
  # A. Switch 대신 Routes를 사용
    * Switch의 이름이 Routes로 변경되었다.
    * exact 옵션 삭제
    * component 렌더링 방식 변경
    * path를 기존 path="/web/:user_id"에서 path=":id"로, 상대경로로 지정한다.
    * 이 외에도 path="." 또는 path=".." 등으로 상대경로를 표현함
    
 ![image](https://user-images.githubusercontent.com/94499416/162664713-9425e041-6868-4469-b19a-a5e2a99cf807.png)
 
    * exact를 사용하지 않고 여러 라우팅을 매칭하고 싶은 경우 URL 뒤에 *을 붙여 사용합니다.
    * component 대신 element를 사용해 바로 component를 전달합니다.
    
  # B. 중첩 라우팅 (outlet) 컴포넌트 사용
  
  ![image](https://user-images.githubusercontent.com/94499416/162665927-63c65da6-a41c-4b34-aa8a-2ff0e2c0c901.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/162665994-e41f8b91-57bd-43f9-9a32-e8cf69cd785e.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/162666032-2b844952-3c4c-4d60-9950-7b4093903019.png)
  
    * App.js에서 자식 태그로 중첩하는 라우터를 기재, Header.js에서 Outlet 라이브러리를 통해 가져온다.
    * v6에선 exact를 안쓰는 대신 /*이 필수
    
  # B. 중첩 라우팅 곧바로 기재

  ![image](https://user-images.githubusercontent.com/94499416/162666500-7994fb79-90bb-4691-b509-4140368d9f64.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/162666525-37c6298f-d682-45fd-8991-25fa63e3886d.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/162666032-2b844952-3c4c-4d60-9950-7b4093903019.png)
  
    * 위 코드와 같이 Outlet 없이 바로 Routes, Route로 기재할 수 있다.
    
  # C. useHistroy대신 useNavigate를 사용

  ![image](https://user-images.githubusercontent.com/94499416/162697179-70dd2cf2-ecf6-490e-924d-556922eed6bc.png)
  
    * v6부터 navigate라는 메소드를 사용한다. history.push()와 history.replace() 모두 navigate라는 명칭을 이용한다.
    * replace 기능이 필요하다면, navigate(to, {replace : true}) 형태로 사용한다.
    * state를 사용한다면, navigate(to, {state])의 형태를 이용한다. to는 <Link>에서 사용한 경로를 넣으면 된다.
    
  ![image](https://user-images.githubusercontent.com/94499416/162698216-aa63ee78-ef10-4111-a617-d533cc739754.png)
  
    * useHistory중 { go, back, goForward } 해당 위치, 이전, 다음으로의 기능을 해왔다. 이 부분도 navigate로 통일하고 index를 넣어 동일한 역할을 수행한다.
