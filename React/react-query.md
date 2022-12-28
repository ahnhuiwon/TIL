## React-Query

### React-Query란?

데이터 Fetching, 캐싱, 동기화, 서버 쪽 데이터 업데이트 등을 쉽게 만들어주는 리액트 라이브러리이다.

<br />

### React-Query의 존재 이유?

기존 Redux, Recoild등 다양하고 좋은 state 관리 라이브러리가 있지만

위 라이브러리는 클라이언트 쪽의 데이터를 관리하기에 적합하지 서버의 데이터들을 관리하기에는

적합하지 않은 점들이 있다. 이 부분을 해결하기 위해 React-Query가 만들어졌다.

<br />

### React-Query의 장점

- React 어플리케이션 내에서 데이터 패칭, 캐싱, 동기 그리고 서버의 상태 업데이트를 좀 더 용이하게 해준다.
- 기존에는 직접 만들어야했던 기능들을 별도의 옵션으로 지원해 React-Query 로직을 통해 짧은 코드로 대체할 수 있다.
- 2번째 이유로 프로젝트 구조가 기존보다 단순해지면서 유지보수하기 쉽고, 새로운 기능을 쉽게 구축할 수 있다.

이 외에도 많은 장점이 있는데

- 별도의 설정 없이 즉시 사용 가능한 점.
- 캐싱을 효율적으로 관리해준다.
- 같은 데이터에 대한 여러번의 요청이 있을 경우 중복을 제거한다.
- 백그라운드에서 오래된 데이터를 알아서 업데이트 해준다.
- 데이터 업데이트가 발생시 최대한 빨리 반영한다.
- 페이징 처리 및 지연 로딩 데이터 같은 성능을 최적화해준다.
- 서버 쪽 데이터를 garbage 컬렉션을 이용해 자동으로 메모리를 관리해준다.
- 구조적 공유를 통해 쿼리의 결과를 기억한다.

<br />

### React-Query 사용하기

아래 명령어를 실행해 React Query를 설치해준다.

```
yarn add react-query
npm install react-query
```

<br />

#### QueryClientProvider

QueryClientProvider는 리액트 어플리케이션에서 비동기 요청을 처리하기 위한 ContextProvider로 동작하며

하위 컴포넌트에서 QueryClient를 사용할 수 있게 해준다.

index.jsx 파일에 다음과 같이 설정해준다.

```
...
import { QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient();
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

<br />

#### QueryCache

QueryCache는 React Query를 이용해, 사용된 쿼리의 메타 정보와 상태등의 데이터를 저장하는 용도로 사용된다.

또한 onError, onSuccess 콜백을 사용해 어플리케이션 전역에서 이벤트를 핸들링할 수 있다.

```
...
import { QueryCache, QueryClient, QueryClientProvider } from 'react-query';

const queryClient = new QueryClient({
  queryCache : new QueryCache({
    onError : (err, query) => {
      console.log(err);
    },
    onSuccess : (data) => {
      console.log(data);
    }
  }) 
});
const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```

<br />

#### useQuery

useQuery는 서버에서 데이터를 가져오기 위해 사용하는 hook이다. 쉽게 get메서드라고 생각하면 된다.

파라미터로 unique key, promise 기반의 함수, 옵션이 들어가며

유니크 키는 애플리케이션 전역에서 해당 쿼리를 refetching, caching, sharing 하는 용도로 사용된다.

useQuery의 리턴 값으로는 status, data, error와 같은 템플릿을 포함하며 데이터 사용에 필요한 정보가 제공된다.

<br />

- isLoading (쿼리에 데이터가 없고 fetching 하는 상태)
- isError (쿼리에 에러가 발생한 상태)
- isSuccess (쿼리가 성공적으로 실행, 데이터를 사용가능한 상태)
- isIdle (쿼리를 사용할 수 없는 상태)
- error (쿼리가 isError일때 에러 정보 확인을 위해 사용하는 프로퍼티)
- data (쿼리가 isSuccess일때 데이터 사용을 위한 프로퍼티)
- isFetching (쿼리의 fetching / refetching 여부에 대한 boolean 값)

아래 코드를 통해 useQuery의 사용 방법을 보자. useQuery에 사용한 파라미터는 다음과 같다.

- queryKey (쿼리에 사용할 유니크 키 값)
- queryFn (쿼리에 사용할 promise 기반의 비동기 함수)
- options (쿼리에 사용할 옵션 값)

```
...
import { useQuery } from 'react-query';
import axios from 'axios';

function App() {

  const api_function = () => {
    return axios.get(`https://jsonplaceholder.typicode.com/users`);
  }

  const { status, data, error } = useQuery('users', api_function, {
    staleTime : Infinity
  });

  if(status === "loading") {
    return <span>Loading...</span>
  }
  if(status === 'error') {
    return <span>Error : {error.message}</span>
  }
  if(status === 'success') {
    return console.log(data)
  }
  
  return (
    <div className="App">
      
    </div>
  );
}
```

<br />

옵션값으로 

staleTime : Infinity는 만료시간을 무한으로 설정하여 API 추가 호출을 방지한다.

아래는 실행한 결과이다.

<br />

![image](https://user-images.githubusercontent.com/94499416/209779718-220d229b-9785-4442-a54f-2625776b7d10.png)
