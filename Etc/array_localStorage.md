# JavaScript
  ## localStorage에 배열 데이터 저장하기
  
    * 프로젝트중 현재 날짜를 고정으로 두고 배열 형식에 데이터를 로컬에서 가져오면서 저장할 필요가 생겼다.    
    * 하지만 localStorage 또는 sessionStorage는 문자열로 데이터가 저장이 된다. Storage 데이터를 배열로 가져오고 배열로 사용하는 방법은 없을까?
    
    
  ## localStorage 데이터를 가져와 null값 구분하기
  
  ![image](https://user-images.githubusercontent.com/94499416/161205567-5d07e378-a2f0-4eb3-9b56-e290a42d39fe.png)

    * localStorage는 문자열로 저장이 되기떄문에 JSON.parse를 사용해 JavaScript 값이나 객체로 생성해준다.
    * localStorage에 저장된 데이터를 가져와 변수에 할당해준다.
    * 조건문을 통해 localStorage 값이 null이라면 새로운 공배열([])을 선언해준다.
    
    
  ## array.push() 후 localStorage.setItem()로 저장하기
  
  ![image](https://user-images.githubusercontent.com/94499416/161205621-de89fd9f-aace-4ed4-92f3-73534d9fc67e.png)

    * 배열 형식이 할당된 변수에 특정 데이터를 push한다. 
    * localStorage.setItem 메소드를 이용해 데이터를 다시 넣어준다.
  
  
  ## localStorage에 데이터가 잘 저장되는 모습
  
  ![image](https://user-images.githubusercontent.com/94499416/161206013-af0b3a3e-2198-45d7-b3bf-3fe25b46635b.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/161206076-ab41a27a-d9d1-4c77-8be3-41bddfbb005e.png)
  
  ![image](https://user-images.githubusercontent.com/94499416/161206191-97adaffb-5d47-46f5-abcb-1d94b297c8c9.png)
