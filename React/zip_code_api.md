# React
  ## 카카오 우편번호 api 사용하기

  * 왜 카카오인가?
    * 다음 우편번호가 너무 간편하고 사용하기 편리하기 때문에 카카오를 선택했다.

  
  ## npm 설치.

  <img width="520" alt="install" src="https://user-images.githubusercontent.com/94499416/166419648-f40603cf-741c-48a2-8bf4-a51b7217b75b.png">
  
    * DaumPostCode의 onComplete 이벤트로 onCompletePost 함수를 호출한다.
    * onComplete는 우편번호 검색 결과 목록에서 특정 항목을 클릭한 경우, 해당 정보를 받아서 처리할 콜백 함수를 정의하는 부분이다.


  ## onCompletePost 함수 호출
  
  <img width="919" alt="function" src="https://user-images.githubusercontent.com/94499416/166421668-d0137063-dd5d-40e8-9bf1-2c1eb50544c2.png">
    
    * 함수의 파라미터로 data를 전달 받는다.
    * 해당 객체 값이 있다면 extraAddr에 데이터를 넣고 최종적으로 fullAddr에 모든 주소를 담는다.
    * state값을 변경하는 함수를 사용해 우편 번호와 주소를 변경해준다.

  ## 변경된 state
  
  ![image](https://user-images.githubusercontent.com/94499416/166422155-ed2bf157-0a68-4a4f-925a-6aeaefafe239.png)
  
    * 빈 문자열('')이었던 state값이 성공적으로 변경된 모습이다.
