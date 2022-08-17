## `Carousel 구현하기`

### `Carousel이란?`

캐러셀은 회전목마란 뜻으로 계속해서 빙빙 돌아가기 때문에 Carousel으로 명명했다.

새로운 프로젝트 중 기획자로부터 계속 돌아가는 캐러셀 기능을 요청했고

계속해서 구글링 해봤지만 버튼을 클릭해서 슬라이드화되는 캐러셀 예제는 많았지만...

사용자가 어떠한 행동을 하지 않아도 자동으로 움직이는 캐러셀 예제는 많지 않아 기록하게 되었다.

<br />

### `전체 코드`

React에서 요구하는대로 DOM 객체에 접근을 최대한 피하면서 state(상태값)을 활용해 구현했다.

<br />

```

const [margin_value, set_margin_value] = useState(410);
    var client_circle;

    useEffect(()=>{
        window.addEventListener('scroll', handle_scroll);

        return ()=>{
            window.removeEventListener('scroll', handle_scroll);
        }
    }, []);

    // margin_value reset useEffect
    useEffect(()=>{
        if(margin_value < -300){
            set_margin_value(410);
        }
    }, [margin_value])

    const start_carousel = () => {
        client_circle = setInterval(()=>{ set_margin_value(margin_value => margin_value - 1) }, 100)
        window.removeEventListener('scroll', handle_scroll);
    }

    // auto carousel function
    const handle_scroll = async (event) => {
        const my_scroll = event.srcElement.scrollingElement.scrollTop;
        if((my_scroll > 3700) && (my_scroll < 4200)){
            const auto_interval = setTimeout(start_carousel, 300)
        }
        if(my_scroll > 1500){
            clearInterval(client_circle);
        }
    }

```

<br />

### `useEffect로 이벤트 생성`

addEventListener 메소드를 이용해 사용자가 스크롤 할 경우 handle_scroll이란 함수를 실행시킨다. 

<br />

```
useEffect(()=>{
        window.addEventListener('scroll', handle_scroll);

        return ()=>{
            window.removeEventListener('scroll', handle_scroll);
        }
    }, []);
```

<br />

### `handle_scroll 함수`

이 함수에서는 사용자의 현재 스크롤 위치를 가져와 특정 조건을 만족할시에 

setTimeout 함수로 start_carousel 함수를 딱 한번만 실행시킨다.

사용자가 캐러셀 영역을 벗어난다면 clearInterval을 통해 함수를 종료한다.

**setTimeout, setInterval등의 코드는 메모리를 많이 잡아먹는다.**

<br />

```

const handle_scroll = async (event) => {
    const my_scroll = event.srcElement.scrollingElement.scrollTop;
    if((my_scroll > 3700) && (my_scroll < 4200)){
        const auto_interval = setTimeout(start_carousel, 300)
    }
    if(my_scroll > 1500){
        clearInterval(client_circle);
    }
}

```

<br />

### `캐러셀 시작 함수`

setInterval 함수를 통해 0.1초마다 상태값을 -1 차감한다.

시간이 지날수록 캐러셀 속도가 빨라지는 문제점이 있었는데 분석해보니 사용자가 계속해서 스크롤시 handle_scroll이 실행되고

스크롤의 위치가 조건에 만족한다면 start_carousel을 다시 실행하게 되는 문제점이 있었다.

캐러셀 시작 함수가 호출되고 호출되고 또 호출되서 속도가 매우 빨라지는것이었다.

해결방법으로는 캐러셀이 시작되면 removeEventListener을 사용해 스크롤 이벤트를 삭제해줘야 다시 함수가 호출되는 대참사가 벌어지지 않는다.

<br />

```
const start_carousel = () => {
    client_circle = setInterval(()=>{ set_margin_value(margin_value => margin_value - 1) }, 100)
    window.removeEventListener('scroll', handle_scroll);
}
```

<br />
