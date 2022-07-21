## useMemo

### `useMemo란 무엇인가?`

렌더링 최적화를 위해 사용하는 react hook.

### `렌더링 최적화`

react는 상위 컴포넌트로 받는 state와 props가 변동될 경우 리렌더링된다.

이 방식은 문제가 존재하는데

> state와 props가 여러개 존재한다면?
> state 하나만 변경을 했는데 다른 state들도 재계산된다면 이 상황을 좋은 리렌더링인가?

useMemo와 useCallback은 이 문제점을 생각한 개발자들을 위해 구현됬다고 보면 된다.
