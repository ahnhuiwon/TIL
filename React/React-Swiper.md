# React-Swiper
  ## Swiper

  * 프로젝트중 고객이 탭메뉴로 구성된 화면을 스와이프로 넘기고 싶다는 요청이 들어와 컴포넌트를 스와이프 할 수 있는 slick과 swiper중 swiper를 선택했다. 
  * vanilla JavaScript에서 사용하는것과 조금 다르기 때문에 React에서도 Swiper를 사용해 보려고 한다.
  * 아래는 공식 사이트이다. 자세한 내용이 궁금하다면 링크를 클릭해보자. 
  * <a href="https://swiperjs.com/react">https://swiperjs.com/react</a>

  ## A. Swiper 설치하기
    npm install --save swiper
    
  ## B. 컴포넌트에 적용시키기
  ![image](https://user-images.githubusercontent.com/94499416/162880423-147fdd01-1ad3-4fec-a464-be0b024f1f09.png)
  
    필요한 컴포넌트들과 컴포넌트에 맞는 style을 import한다. 모두 swiper.js에서 제공하니까 걱정할 필요는 없다.
    Swiper.use를 import 한 뒤, use([]) 비어있는 배열안에 자기가 사용할 옵션들을 넣는다.
    Swiper 컴포넌트의 props로 넣어준다.
    
  
  ## C. Swiper Index 가져오기
  
  단순 스와이퍼면은 상관이 없겠지만 스와이퍼로 변경된 화면이 탭메뉴에서도 사용자가 어디에 위치했는지 네비게이션이 필요했다.
  Swiper에서 화면마다 index를 주는 기능이 있다고해서 그 기능을 이용해 탭메뉴를 변경할려고 한다.
  
  ![image](https://user-images.githubusercontent.com/94499416/162880964-08637237-7057-47b2-8270-1ac9f627209f.png) <br />
  ![image](https://user-images.githubusercontent.com/94499416/162881174-741d9ba2-c693-47ff-af89-6a514d254946.png)
  
  Swiper의 기능을 사용하기 위해 해당 컴폰전트에 ref를 추가한다.
  매개변수(swiper_params)에 onSlideChange 이벤트가 핸들러가 실행된다면 이벤트 핸들러의 이벤트를 받는다.
  이벤트 핸들러의 activeIndex 값을 받아 탭메뉴를 변경하는 nav_mode(e.actriveIndex)라는 메소드에 파라미터로 값을 전달해준다. 
  
  ## D. 탭메뉴 변동시에도 swiper 적용하기
  
  ![image](https://user-images.githubusercontent.com/94499416/162882842-b83b2283-7f04-42de-a663-6a954a8173a2.png)
  
  탭메뉴 상태값을 변경하는 메소드를 실행한다. 파라미터로 변경할 상태값을 넣어준다.
  
  ![image](https://user-images.githubusercontent.com/94499416/162883016-f631ff4a-02ac-4eb6-84eb-56db76f25f93.png)
  
  전달받은 파라미터 값을 삼항 연산자를 통해 맞는 조건을 수행한다.
  swiper 컴포넌트를 ref로 잡아 해당 슬라이드 페이지로 이동하는 slideTo() 메소드에 사용자가 이동하고 싶은 swiper index를 넣는다.
