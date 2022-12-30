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

#### Query Cache Key

queryKey는 React Query에서 중요한 개념이다. 내부적으로 데이터를 캐시하고 쿼리에 대한 종속성이 변경될때 자동으로 다시 가져올 수 있게 한다.

그리고 필요한 시점에 queryKey를 통해 query cahce와 상호작용이 가능하다.

<br />

**Cashing Data**

내부에서 query cache는 Key가 직렬화되어 있고, Key는 해쉬되어 관리된다.

아래 코드는 오브젝트의 키 순서와 관계없이 다음 쿼리는 모두 같은 쿼리로 취급한다.

```
useQuery(['todos', { status, page }], ...)
useQuery(['todos', { page, status }], ...)
useQuery(['todos', { page, status, other: undefined }], ...)
```

<br />

아래 코드는 배열 요소 순서에 의해 같지 않은 쿼리 키이다.

```
useQuery(['todos', status, page], ...)
useQuery(['todos', page, status], ...)
useQuery(['todos', undefined, page, status], ...)
```

<br />

중요한 사실은 Key가 쿼리에 대해 유니크 해야한다는것이며

React Query는 cache에 Key를 이용해 접근한다는 것이다. 당연히 useQuery 및 useInfiniteQuery에 동일한 Key를

사용할 수 없다. 결국 하나의 query cache만 유효하게 된다.

```
useQuery(['user'], api_function)

// 잘못된 사용
useInfiniteQuery(['user'], api_function)

// 사용 가능
useInfiniteQuery(['infinite_user'], api_function)
```

<br />

**staleTime과 cacheTime**

<br />

**자동 Refetch**

refetching의 전제 조건은 데이터가 stale한 상태, 즉 캐싱된 data를 말하며 최신화가 필요한 데이터를 말한다.

쉽게 말한다면 최신화가 필요한 상태가 되면 refetch 된다는 것이다.

refetch 되는 조건들은 아래와 같다.

1. query key에 react state를 포함시키고, state값이 변경될때
2. 데이터가 stale일 경우, 윈도우에 포커스가 될떄
3. 마운트 될때마다
4. 연결이 끊어졌다가 재 연결 되었을 때
5. 고의로 쿼리 무효화를 했을 때
6. 명시적으로 refetch 함수를 호출

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

<br />

**useQuery refetching**

아래 코드는 위에서 언급한 1번 state를 활용한 refetching 방법이다.

```
function App() {

  const [first_state, set_first_state] = useState(1);


  // state 변경 함수
  const change_state = (value_param) => {
    const { value } = value_param.target;

    set_first_state(value);
  }


  // api 함수
  const api_function = async (user_param) => {
    const res_data = await axios.get(`https://jsonplaceholder.typicode.com/users/${user_param}`);

    return res_data.data
  }


  // react-query
  const { data } = useQuery(['users', first_state], () => api_function(first_state));
  // fetchComments는 postId를 인자로 받기 때문에 익명함수로 감싸서 작성한다
  // 쿼리 키를 의존성 배열로 작성하여 post.id마다 각기 다른 쿼리를 생성
  
  
  return (
    <div className="App">
      <input type="number" value={first_state} max="10" onChange={(e)=>{ change_state(e) }} />
      {
        data &&
        <ul>
          <li>{data.username}</li>
          <li>{data.email}</li>
          <li>{data.phone}</li>
          <li>{data.website}</li>
        </ul>
      }
    </div>
  );
}
```

<br />

![image](https://user-images.githubusercontent.com/94499416/209924705-6bf46b47-829a-4a0f-bc97-e0757239b640.png)

<br />

![image](https://user-images.githubusercontent.com/94499416/209924736-f3b94ed6-6d51-4c75-98de-3bb9762de648.png)

<br />

state값이 변경됨에 따라 refetching되는 화면이다.

<br />

**useQuery 동기적으로 사용하기**

useQuery는 기본적으로 비동기로 동작한다. useQuery에 후에 서술할 코드와 같이 enabled 옵션을 false로 사용하면 동기적으로 사용할 수 있다.

enabled 옵션을 false로 설정한다면 컴포넌트가 마운트 되거나 윈도우 포커스가 되어도 쿼리가 자동으로 실행되지 않는다.

또한 queryClient에서 invalidateQueries, refetchQueries 함수를 호출해도 refetching되지 않는다.

쿼리는 캐싱되지 않은 idle 상태이며 fetching을 위해서는 refetch 함수를 트리거로 사용해야 한다.

아래 코드는 버튼을 클릭시 useQuery를 통해 데이터를 가져오는 코드이다.

```
function App() {

  const [first_state, set_first_state] = useState(1);

  // api 함수
  const api_function = async (user_param) => {
    const res_data = await axios.get(`https://jsonplaceholder.typicode.com/users/${user_param}`);

    return res_data.data
  }

  // react-query
  const { status, data, refetch, error } = useQuery(['users', first_state], () => api_function(first_state),{
    enabled : false
  });
  
  if(status === "success"){
    console.log(data)
  }

  if(status === "error"){
    console.log(error)
  }

  useEffect(()=>{
    console.log(status);
  }, [status])

  return (
    <div className="App">
      <button onClick={()=>{ refetch() }}>USER</button>
    </div>
  );
}
```

<br/>

실행 결과는 다음과 같다.

![image](https://user-images.githubusercontent.com/94499416/210027358-32b8cb49-ea46-42ac-987c-56471b8c793d.png)

<br />


