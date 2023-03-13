## `구글 로그인 구현하기`

1차적으로 'react-google-login' 패키지를 통해 구현하였다.

일정 시간이 지나고 구글 로그인을 시도하자 response로 받아온 데이터 중 일부 값이 없어 에러를 일으켰다.

에러 당시에는 구글링을 시도해도 해결책으로 보이는 정보가 없어 패키지를 변경하는 방법으로 수정했다.

글을 작성하는 현재는 아래 작성할 두 이유 때문에 발생한 에러 였는데

1. gapi 패키지의 코드 업데이트

2. react-google-login 패키지가 더 이상 사용되지 않는 문제

<br />

### `react-google-login의 지원 중단`

![image](https://user-images.githubusercontent.com/94499416/224594637-993afa08-8dd9-49d8-81e1-56d35a2c2e02.png)

<br />

말 그대로 웹용 Google Sign-In Javascript Platform Library가 중단될 예정이라 새로운 웹용 Google ID 서비스 SDK를 사용해야 한다고 한다.

OAuth 웹 클라이언트 ID를 통해 로그인을 하는 방법으로 업데이트 되었으며 발급받는 방법과 초기세팅은 구글에 이미 많으니 패스하겠다.

<br />

### `react-oauth/google 패키지 사용`

react-oauth/google 패키지를 사용함으로써 구글 소셜 로그인을 다시 구현하였다.

![image](https://user-images.githubusercontent.com/94499416/224596042-e343cf53-d136-4525-842a-0799e3ddd29a.png)

<br />

먼저 패키지를 import해서 구글 로그인 컴포넌트를 갖고온다.

<br />

![image](https://user-images.githubusercontent.com/94499416/224596122-598c9826-e4c2-4265-8283-80c0cf19fd9e.png)

<br />

clientId에는 구글에서 발급 받은 OAuth 웹 클라이언트 ID를 넣어주면 된다.

onSuccess는 구글 로그인이 성공했을때 그 이후 작업을 설정해주면 된다.

또 하나의 특이점으로는 구글 로그인을 한 유저 정보가 바로 response 값으로 들어오는게 아니라

jwt(Json Web Token)을 response 값으로 주고 jwt-decode 라는 패키지를 통해 디코딩을 해야 유저 정보를 알 수 있다.
