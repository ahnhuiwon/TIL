# 코드 스플리팅
    
<br />    
    
React 프로젝트는 클라이언트에게 제공할때 빌드 작업을 거쳐서 배포를 한다.
  - 프로젝트내 불필요한 주석, 경고메세지, 공백을 제거하여 파일 크기를 최소화을 위해
  - JSX 문법이나 최신 자바스크립트 문법이 원활하게 실행되도록 프랜스파일 작업을 위해
  - 프로젝트 내, 이밎와 같은 정적 파일이 있다면 경로 설정
    
<br />
    
이 작업은 웹팩이라는 도구가 담당하는데 별도의 설정을 하지 않는다면 프로젝트에서 사용 중인
    
모든 자바스크립트 파일과 CSS 파일이 하나로 합쳐진다.
    
CRA(create-react-app)으로 빌드할경우 최소 두 개 이상의 자바스크립트 파일이 생성된다.
    
<br />

> 🤨 CRA 기본 웹팩 설정에는 SplitChunks라는 기능이 적용되어 node_modules에서 불러온 파일, <br />
> 일정 크기 이상의 파일, 여러 파일 간에 공유된 파일을 자동으로 따로 분리시켜 캐싱의 효과를 제대로 누릴 수 있게 해준다.

<br />    
    
![Untitled](https://user-images.githubusercontent.com/94499416/185858311-003823d4-9f63-48ca-96fb-6eac7a7d6c1f.png)
    
<br />    
    
위는 CRA로 리액트 프로젝트를 생성한 뒤 build 디렉토리 구조이다.
    
- ‘a19297df’같은 해시(hash)값이 포함되어 있고 이 값은 빌드하는 과정에서 해당 파일의 내용에 따라 생성된다.
    
이를 통해 브라우저가 새로 파일을 받는 유무를 알 수 있다.
    
- 787로 시작하는 파일에는 React, ReactDOM과 node_modules에서 불러온 라이브러리 코드가 있다.

<br />

> 🤨 위에서 언급했던 SplitChunks라는 기능을 통해 자주 바뀌지 않는 코드들이 787로 시작하는 파일에 들어있어 
> 캐싱의 이점을 더 오래 누릴 수 있다는 장점이 있다.


<br />

내용을 간단하게 변경하고 다시 build를 해보자.
    
![Untitled (1)](https://user-images.githubusercontent.com/94499416/185858318-fd5ad7b4-8318-4c84-bdcb-ba245c5323e0.png)
    
<br />    
    
node_modules에서 불러온 라이브러리가 들어있던 787로 시작하는 파일의 이름은 바뀌지 않고
    
컴포넌트 관련 코드가 들어 있던 main으로 시작하는 파일의 이름은 변경된 것을 확인할 수 있다.

<br />

---
    
## 🤔 왜 코드 스플리팅이 필요할까?
    
<br />    
    
CRA 프로젝트에 기본으로 탑재되어있는 SplitChunks 기능을 통한 코드 스플리팅은 단순히
    
효율적인 캐싱 효과만 있을 뿐이다.
    
만약 로그인, 아이디 찾기, 비밀번호 찾기가 구성된 SPA를 개발한다고 가정해보자.
    
사용자가 로그인 페이지를 방문했다면 아이디 찾기와 비밀번호 찾기 페이지에서 사용된
    
컴포넌트 정보는 필요하지 않다. 사용자가 실제로 해당 페이지를 이동할때만 필요하기 때문이다.
    
하지만 리액트 프로젝트에서 별도로 설정하지 않는다면 로그인, 아이디 찾기, 비밀번호 찾기 페이지 코드는
    
모두 한 파일에 저장되어 버리기 때문에 어플리케이션의 규모가 커진다면 필요하지 않은 코드들도
    
모두 불러오면서 로딩이 오래 걸리고 트래픽도 많이 나오는 등 문제가 생길 수 있기 때문이다.

### 함수를 스플리팅하는 방법
    
<br />    

아래 코드는 Hello React를 클릭하면 notify 함수가 실행되는 코드이다.

```

// notify.jsx

const notify = () => {
    alert('잘못된 접근입니다.');
}

export default notify;

```

<br />   

```

// App.js

import logo from './logo.svg';
import './App.css';
import notify from './components/notify';

function App() {

  const alert_func = () => {
    notify();
  }

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={alert_func}>Hello React</p>
      </header>
    </div>
  );
}

export default App;

```

<br />   

아래 코드는 위 코드를 스플리팅한 코드이다. 

<br />   

```

// App.js

import logo from './logo.svg';
import './App.css';

function App() {

  const alert_func = () => {
    import('./components/notify').then(( result )=>{ result.default() });
  }

  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={alert_func}>Hello React</p>
      </header>
    </div>
  );
}

export default App;


```

<br />  

코드를 스플리팅 한 뒤 빌드를하면 notify 코드가 main 파일 안에 들어가게 된다.

하지만 위 코드와 같이 상단에서 import를 하지 않고 import() 함수 형태로 메서드 안에서 사용한다면

파일을 따로 분리시켜서 저장하는 특징이 있다.

import 함수를 사용한다면 Promise를 반환하는데 import 함수로 사용하는 문법은 아직 표준 자바스크립트에 해당하지 않지만

현재 웹팩에서 지원하고 있어 별도의 설정 없이 프로젝트에 바로 사용할 수 있다.

그리고 import() 함수를 통해 모듈을 불러올때 default로 내보낸것은 result.default를 참조해야 사용할 수 있다.
