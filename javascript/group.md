# filter와 reduce을 사용해 그룹핑 후 요소 가져오기

## 전체 코드

전체적인 코드는 아래와 같다.

```
const site_group_keyword = props.data.filter.reduce((acc, curr) => {
  const { site } = curr;
  if (acc[site]) acc[site].push(curr);
  else acc[site] = [curr];
  return acc;
}, {});
```

<br />

이 코드에서 사용하는 reduce 파라미터 acc, curr은 다음과 같다.

-acc
  - accumulator : callback 함수의 반환값을 누적
- curr
  - currentValue  : 배열의 현재 요소

<br />

그룹핑한 결과를 담을 acc와 현재 요소 값 curr 파라미터를 가져오고

위 코드에서는 각 사이트에 따라 그룹화를 하므로 curr(현재 요소 값)의 site 값을 가져온다.

조건문을 통해 acc에 site에 해당하는 값이 있으면 현재 요소의 값을 추가하고

해당되는 값이 없다면 없는 사이트를 key로 생성한 뒤 curr 값을 추가한다.

누적된 acc를 return 해주고 acc는 객체 형태가 되어야 하기 떄문에 초기값은 {}로 선언한다.

<br />

## 그룹핑한 요소 가져오기

```
let button_ui = Object.keys(site_group_keyword).map((data, index) => {
    return (
      <Button
        key={index}
        onClick={() => {
          temp_func(data);
        }}
      >
        {data}
      </Button>
    );
  });
```

<br />

그룹핑한 site_group_keyword는 object 타입이기 떄문에 Object.keys()를 사용해 array 타입으로 변환해주고 반복문을 돌린다.
