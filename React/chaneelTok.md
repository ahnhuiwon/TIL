# React
  ## React 채널톡 문의 기능을 연동하기

  자사 홈페이지 구축중에 채널톡을 연동해 달라는 기획자분의 요청이 왔다.

  예제도 그렇게 많지 않았고 짧은 시간동안 삽질을 했지만 향후 동일한 기능 요청이 왔을때
  
  잊어버리지 않게 기록할려고 한다.
  
  <br />
  
  ### 채널톡 컴포넌트를 생성하기.

  ![tok1](https://user-images.githubusercontent.com/94499416/184269445-45bd7de9-04a1-44ee-bd32-07be9d250dae.PNG)
  
  컴포넌트를 하나 생성하고 위 코드를 넣어준다.
  
  **주의사항으로는 절대 import 한 뒤 `<ChannelTok />`처럼 불러오지 않는다.**
  
  **코드를 보면 알겠지만 jsx가 없어 모듈을 불러와 태그처럼 사용한다면 에러를 출력한다.** 

  <br />

### 설정 코드를 라우터에 추가
  
![tok2](https://user-images.githubusercontent.com/94499416/184270897-2242d4ed-6eb8-412b-8c1d-b72d1c1ee85d.PNG)
    
라우터에 위 코드를 추가한다.
    
boot() 메소드내에 들어가는 파라미터는 다음과 같다.
    
 ```
    
ChannelService.boot({
  "pluginKey": "pluginKey", //please fill with your plugin key
  "memberId": "유저ID",
  "profile": {
    "name": "유저Name",
    "email": "유저Email", 
    "id": "유저ID"
   }
});
    
```
    
<br />

### pluginKey 확인하기
  
![tok3](https://user-images.githubusercontent.com/94499416/184272268-81cbf81e-4d65-4a12-93e3-54e3da550467.jpg)
  
플러그인키는 로그인 후 위 이미지 경로에서 확인할 수 있다.
