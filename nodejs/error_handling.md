# nodeJs error handling

## 예외 처리하기

노드의 메인 스레드는 하나인 싱글 스레드이기 때문에, 에러가 나면 스레드를 갖고 있는 프로세스가 멈춘다는 뜻이고,

전체 서버도 멈춘다는 뜻과 같기 때문에 에러를 잘 처리해야한다.

콜백함수 같은 곳에 if문, Promise에서의 catch문, async 구문에서의 try, catch를 통해 에러를 처리해야한다.

<br />

**try catch**

```
setInterval(()=>{
  console.log('run');
  
  try {
    throw new Error('에러');
  } catch(err) {
    console.log(err);
  }
})
```

<br />

**Promise**

new Promise((resolve, reject) => {
  throw new Error('에러')
})
.then((resolve) => { console.log(resolve) })
.catch((err)=> { console.log(err) })

<br />

**uncaughtException**

예측 불가능한 에러를 처리할떄 사용한다.

```
process.on('uncaughtException', (err) => {
  console.log('예기치 못한 에러', err);
});

setInterval(() => {
	throw new Error('새로운 에러');
}, 1000);
 
setTimeout(() => {
	console.log('실행됩니다');
}, 2000);
```

<br />

다만 uncaughtException는 이벤트 발생 이후 다음 동작이 제대로 동작하는지 보증하지 않는다.

즉 복구 작업 코드를 넣어두었다고 해도 그것이 동작하는지는 확신할 수 없다는것이다.

따라서 단순히 에러 내용을 기록하는 정도로 사용하고, 에러를 기록한 뒤 process.exit()로 프로세스를 종료하고

서버를 재시작하는 것이다.
