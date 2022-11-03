## `React에서 카카오맵과 주소를 위도, 경도로 변경하기`

### `카카오 api 설정하디`

https://developers.kakao.com/

먼저 위 사이트에서 회원가입을 한 뒤 내 어플리케이션에서 어플리케이션을 추가한다.

![image](https://user-images.githubusercontent.com/94499416/199677552-a943c1f2-e9b9-4b2e-8508-ef39070b3ef9.png)

<br />

앱 키를 클릭한다면 위와 같은 화면을 확인 할 수 있다.

<br />

저 중에 웹으로 카카오 맵을 띄워야하기 때문에 javascript 키를 활용한다.

<br />

![image](https://user-images.githubusercontent.com/94499416/199677925-2cff43a7-c646-49e0-adc8-ab8bfe18353a.png)

<br />

왼쪽 메뉴에서 플랫폼을 클릭한 뒤 Web에 카카오 맵을 사용할 도메인을 넣어준다. 아직은 개발 단계이므로 localhost를 넣었다.

### `React 환경설정하기`

```

<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=키입력&libraries=services"></script>

```

public/index.html 파일 <head> 태그안에 위 코드를 넣어준다.
  
appkey에는 javascript 키를 넣어준다.
  
코드에 libraries를 넣지 않는다면 카카오맵을 제외한 부가적인 기능을 사용하지 못하게 된다. (예를 들면 주소를 위도, 경도로 변환하는 기능)
  
주소를 위도와 경도로 변환하는 기능도 필요하니 코드에 포함시킨다.

<br />

### `주소를 위도 경도로 변환하기`
  
```
export const useMaps = (prev_addr) => {

    /** 주소 -> 위도 경도 변환 hook */
    useEffect(()=>{

        const get_code = new kakao.maps.services.Geocoder();

        let callback_func = async (result, status) => {
            if(status === kakao.maps.services.Status.OK){
                const {x,y} = result[0];

                kakao_map_func(x, y);
            }
        };

        get_code.addressSearch(prev_addr, callback_func);

    }, []);


    /** kakao map 생성 함수 */
    const kakao_map_func = (x, y) => {

        const container = document.getElementById("my_map");

        const options = {
            center : new kakao.maps.LatLng(y, x),
            level : 3,
            isPanto : true
        };

        const marker = new kakao.maps.Marker({
            position : new kakao.maps.LatLng(y, x)
        });

        const map = new kakao.maps.Map(container, options);
        marker.setMap(map);
    }
}
```
  
전체적인 코드인데 하나씩 살펴본다.
  
주석과 마찬가지로 위 useEffect는 받아온 주소를 위도와 경도로 변환하는 hooks이며
  
kakao_map_func 함수는 위도와 경도 데이터를 카카오맵으로 보여주는 함수이다.

<br />
  
```
  
useEffect(()=>{

  const get_code = new kakao.maps.services.Geocoder();

  let callback_func = async (result, status) => {
    if(status === kakao.maps.services.Status.OK){
    const {x,y} = result[0];

    kakao_map_func(x, y);
    }
  };

  get_code.addressSearch(prev_addr, callback_func);

}, []);
  
```
  
변수 get_code에 카카오에서 제공하는 기능중 Grocoder를 할당해준다.
  
addressSearch 메소드에 첫번째 인자로 주소, 두번째 인자로 익명함수로 구성된 콜백함수를 넣어준다.
  
콜백함수는 파라미터 두개를 넘겨주는데 첫번째 인자는 위도와 경도 데이터가 담긴 result
  
두번째 인자는 통신 상태를 알려주는 데이터이다. 통신이 성공한다면 status는 OK를 반환한다.
  
status가 OK라면 구조 분해 할당 문법으로 데이터를 할당해준 뒤 kakao_map_func에 파라미터로 전달해준다.
  
<br />

### `카카오맵 출력하기`

```
const kakao_map_func = (x, y) => {
        const container = document.getElementById("my_map");

        const options = {
            center : new kakao.maps.LatLng(y, x),
            level : 3,
            isPanto : true
        };

        const marker = new kakao.maps.Marker({
            position : new kakao.maps.LatLng(y, x)
        });

        const map = new kakao.maps.Map(container, options);
        marker.setMap(map);
}
```

my_map이란 id를 가진 DOM을 갖고와 변수에 할당해준 뒤
 
사용할 옵션들과 위도 경도 값을 넣어준다.
  
마커에도 동일한 위도 경도 값을 넣은 뒤 setMap 메소드에 카카오맵 데이터가 담긴 map을 파라미터로 넣어준다.
  
<br />
