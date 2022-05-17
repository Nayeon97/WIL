## 🚀 useMemo, useCallback (+ React.memo + HOC)

---

작성일: 2022년 5월 16일

    ⭐️ 1차 프로젝트가 끝나고 2차 프로젝트에는 리액트의 렌더링 성능 최적화 관련한 hook을 사용해보겠다! 라는 다짐을 했었는데, 2차 프로젝트때 거의 사용해보지 못했다.
    그 이유에 대해 생각해보니,  익숙하지 않은 기술에 대한 두려움 때문이였는 것 같다.
    공부를 제대로 하지 않은 상태였기에  '과연 잘 쓸 수 있을까', '너무 오버스펙아닌가? ' , '이렇게 하는 것이 맞나? ' 하는 두려움들로
    최적화를 하지 못한 채 2차 프로젝트가 마무리 되었다. 3차 프로젝트에는 제대로 사용해보기 위해 미리 공부하자!
    useMemo, useCallback 에 대하여 알아보자!

### 📖 목차

[1️⃣useMemo란](#1️⃣-usememo란)  
 [useMemo가 빛을 발하는 로직!](#usememo가-빛을-발하는-로직)  
 [✅ 그런데 useMemo 잘 쓸 일이 없다...?](#그런데-usememo-잘-쓸-일이-없다)  
 [2️⃣ useCallback란?](#2️⃣-usecallback란)  
 [⚠️ 잠깐!](#잠깐)  
 [그럼 useCallback도 useMemo 처럼 자주 사용하지 않는 것인가...?](#그럼-usecallback도-usememo-처럼-자주-사용하지-않는-것인가)  
 [+ React.memo ?](#reactmemo)  
 [HOC ... ?](#hoc)  
 [다시 돌아와서 React.memo 에 대해 알아보자](#다시-돌아와서-reactmemo-에-대해-알아보자)[React.memo 비교 방법 수정가능하다!](#reactmemo-비교-방법-수정가능하다)  
 [⚠️ 콜백함수 주의하자!](#⚠️-콜백함수-주의하자)  
 [useCallback, React.memo](#usecallback-reactmemo)  
 [마지막으로 React.memo 언제 쓰면 좋을까?](#마지막으로-reactmemo-언제-쓰면-좋을까)  
 [느낀점](#느낀점)

### 1️⃣ useMemo란?

✔️ 공식문서에 잘 설명이 되어있는데, **메모리제이션된 값을 반환한다라는 문장이 핵심이다.**  
useMemo는 의존성이 변경되엇을 때에만 메모이제이션된 값만 다시 계산한다고 한다.  
쉽게 말해, 맨 처음 값을 캐싱을 해두고 매번 계산을 하는 것이 아니라 캐싱한 값을 꺼내서 사용하는 것!

⚠️ 함수형 컴포넌트가 렌더링 된다는 것 -> 호출된다는 것 -> 호출이 될 때마다 모든 내부 변수가 초기화 된다!

예시

```js
function Component() {
  const value = calculate();
  return <div>{value}</div>;
}

function calculate() {
  return 10;
}
```

Component 라는 함수가 랜더링될 때마다 value 라는 변수는 초기화되고, calculate 함수는 반복적으로 호출된다..!! (매우 비효율적)

🤩 **해결**

```js
function Component() {
  const value = useMemo(() => calculate(), []);
  return <div>{value}</div>;
}
```

✅ **useMemo 구조**

```js
const value = useMemo(() => {
  return calculate();
}), [];
```

✔️ 콜백함수 : 메모이제이션할 값을 계산해서 return  
✔️ 의존성 배열: 배열 안에 있는 요소 값이 업데이트 될때만 콜백함수를 다시 호출해서 메모이제이션할 값을 업데이트해준다! 만약 의존성 배열이 빈 배열이라면 처음에 마운트 될때만! 이후에는 메모이제이션된 값을 return!

### ⭐️useMemo가 빛을 발하는 로직!

```js
import React, { useEffect, useState } from 'react';

function Memo() {
  const [number, setNumber] = useState(0);
  const [isKorea, setIsKorea] = useState(true);

  const location = useMemo(() => {
    return {
      country: isKorea ? '한국' : '외국',
    };
  }, [isKorea]);
  // useMemo 를 활용함으로써, number 값이 변경될 때 location 재렌더링을 막을수 있다.

  useEffect(() => {
    console.log('useEffect 호출');
  }, [location]);

  return (
    <div>
      <h3>하루에 몇끼 드심?</h3>
      <input
        type="number"
        value={number}
        onChange={(e) => setNumber(e.target.value)}
      />
      <hr />
      <h3>지금 어디?</h3>
      <p>나라: {location.country}</p>
      <button onClick={() => setIsKorea(!isKorea)}>비행기 타고 가요~</button>
    </div>
  );
}
```

```js
const location = isKorea ? '한국' : '외국';
```

이때 location 는 원시 타입이기 때문에 number 값이 바뀌어도 useEffect는 실행되지 않는다! number 값이 바뀔 때 마다 location 은 초기화가 계속 되지만, 똑같은 string 을 할당받기 때문에 useEffect 실행 X. (useEffect 는 의존성 배열안에 있는 요소의 내용이 변경되었을 경우에만! 실행된다! )

```js
const location = { country: isKorea ? '한국' : '외국' };
```

객체일때는 얕은 복사! -> 계속 새로운 메모리 공간에 할당되기 때문에 -> 매번 useEffect 함수 호출!

### ✅ 그런데 useMemo 잘 쓸 일이 없다...?

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

이미 공식문서 코드에서도 computeExpensiveValue , 비싼연산으로 기재되어있다.  
그 이유는 useMemo 를 남용하면 오히려 컴포넌트의 복잡도가 올라가고, 코드도 읽기 어려워지고 유지보수성도 떨어지고, useMemo 가 적용된 레퍼런스는 재활용을 위해 garbage collection 에서 제외되기 때문에 오히려 메모리를 더 쓴다고 한다! -> ⭐️ 즉, useMemo 를 쓰는게 이득인지 아닌지 잘 판단해야함!

(+ garbage collection : JS 엔진에서 계속 끊임없이 작동 -> 불필요한 객체 자동 삭제)

참고한 블로그에서는 useMemo가 빛을 발휘할 수 있는 상황은 극히 제한적이라고 한다!

### 2️⃣ useCallback란?

✔️ useCallback은 함수 자체를 memoization하는 hook이다. (useMemo 와 거의 비슷 -> 함수를 메모이제이션한다는 차이!)

```js
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

예시

```js
import { useCallback, useEffect, useState } from 'react';
import Box from './Box';

function Callback() {
  const [size, setSize] = useState(100);
  const [isDark, setIsDart] = useState(false);

  const createBoxStyle = useCallback(() => {
    return {
      backgroundColor: 'pink',
      width: `${size}px`,
      height: `${size}px`,
    };
  }, [size]);

  // createBoxStyle을 useCallback 처리를 해주면서, isDark가 랜더링될 때, createBoxStyle 함수가 재랜더링되는 것을 막음.

  return (
    <div style={{ background: isDark ? 'black' : 'white' }}>
      <input
        type="number"
        value={size}
        onChange={(e) => setSize(e.target.value)}
      />
      <button onClick={() => setIsDart(!isDark)}>Change Theme</button>
      <Box createBoxStyle={createBoxStyle} />
    </div>
  );
}
```

### ⚠️ 잠깐!

useCallback 은 어떻게 사용해야 의미있는 성능 향상을 할지 판단하기 위해서는 일단 자바스크립트 함수의 동등성에 대해 알 필요가 있다고 한다!

```js
const add1 = () => x + y;
// undefined
const add2 = () => x + y;
// undefined
console.log(add1 === add2);
//false
```

⭐️false 가 나오는 이유는 자바스크립트에서 함수도 객체로 취급되기 때문이다!

### 🧐 그럼 useCallback도 useMemo 처럼 자주 사용하지 않는 것인가...?

useCallback 은 useMemo 보다는 사용하는 경우가 꽤 있다고 한다!  
다만, 중요한 것은 사용을 하긴 전에! 성능 테스트를 해보고 사용이 꼭 필요한 경우에만 사용하는 것을 권장한다고 한다!

### + React.memo ?

(useCallback 에 대해 공부하다가 알게 되었다! useCallback 과 함께 자주 쓰인다고 한다.)

✔️ React.memo는 HOC 이다. (Higher-Order Components(HOC) , 컴포넌트를 인자로 받아 새로운 컴포넌트를 return 해주는 함수! )

**그런데 사실 HOC 가 무엇인지 잘 모른다...🥲 일단 HOC 부터 공부해보자..**

### HOC ....?

> 고차 컴포넌트(HOC, Higher Order Component)는 컴포넌트 로직을 재사용하기 위한 React의 고급 기술입니다. 고차 컴포넌트(HOC)는 React API의 일부가 아니며, React의 구성적 특성에서 나오는 패턴입니다.
> **구체적으로, 고차 컴포넌트는 컴포넌트를 가져와 새 컴포넌트를 반환하는 함수입니다.**

공식문서의 설명이다.

사실 잘 이해가 되지 않아서, 다른 설명을 찾아보던 중 고차함수와 거의 유사하다는 글을 발견했다..!!  
고차함수는 다른 함수를 전달인자로 받거나, 함수 실행의 결과를 함수로 반환 함수를 말하는데,
대표적인 예시로 자주 사용하는 메서드인 map() 가 있다.

```js
const nums = [1, 2, 3];
const addNums = nums.map((num) => num + 5);
// 콜백 함수에서의 실행결과를 리턴한 값으로 이루어진 배열을 만들어 반환함.
console.log(addNums); // [6, 7, 8]
```

어떤 느낌인지...감이 아주 살짝 오니 다시 고차컴포넌트로 돌아오자.

```js
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

위의 코드에서 higherOrderComponent 는 고차 컴포넌트, WrappedComponent 는 일반 컴포넌트이다.  
EnhancedComponent 는 말 그대로 고차 컴포넌트가 반환한 향상된 컴포넌트!

```js
function withProps(Comp, props) {
  // HOC 이름 규칙 : with_
  return function (ownProps) {
    return <Comp {...props} {...ownProps} />;
  };
}
```

```js
function Hello(props) {
  return (
    <div>
      Hello, {props.name}. I am {props.myName}
    </div>
  );
}

const HelloJohn = withProps(Hello, { name: 'John' });

const App = () => (
  <div>
    <HelloJohn myName="Kim" />
    <HelloJohn myName="Lee" />
  </div>
);
```

위의 코드를 보고 이해가 되었지만, 확실하지 않다. 직접 사용해보지도 않아서..직접 사용해보면 또 다른 느낌일 것 같다.  
일단 오늘 글에서 HOC 설명은 이정도로만 하고 추후에 !!! 꼭 보충 할 것이다!!

### 다시 돌아와서 React.memo 에 대해 알아보자.

React.memo 는 HOC 이다. 를 읽고 잠시 HOC 에 대해 정리하였다...  
다시 돌아오자!

✅ 간단한 사용방법

```js
export default React.memo(MyComponents);
```

or

```js
export const updateMyComponents = React.memo(MyComponents);
```

React.memo 는 컴포넌트를 렌더링한 뒤에, 이전에 렌더된 결과와 비교하여 DOM 업데이트를 결정한다!  
즉 컴포넌트가 React.memo 로 래핑된다면, 컴포넌트를 렌더링하고 결과를 메모이징 -> 다음 렌더링이 일어날때 `props가 같다면` 메모이징한 내용을 재사용한다!

### React.memo 비교 방법 수정가능하다!

useMemo, useCallback 과 마찬가지로, props 혹은 props의 객체를 비교할때 `얕은 비교` 를 한다! 이미 useCallback 에서 언급되었지만, 함수도 객체로 취급되기 때문이다!

대신 React.memo 는 비교 방식을 수정할 수 있다! 두 번째 매개변수로 비교함수를 만들어 넘겨주면 된다..!!

```js
function MyComponents(props) {}

function areEqual(prevProps, nextProps) {
  /*
  nextProp가 prevProps와 동일한 값 -> true, 다르다면 -> false
  */
}

export default React.memo([MyComponents, areEqual]);
```

### ⚠️ 콜백함수 주의하자!

부모 컴포넌트가 자식 컴퍼넌트의 콜백 함수를 정의한다면, 새 함수가 임시적으로 생성될 수 있다고 한다!  
참고한 블로그에 기재된 설명을 통해 이해해보자!

```js
function Logout({ username, onLogout }) {
  return <div onClick={onLogout}>Logout {username}</div>;
}

const MemoizedLogout = React.memo(Logout);
```

```js
function MyApp({ store, cookies }) {
  return (
    <div className="main">
      <header>
        <MemoizedLogout
          username={store.username}
          onLogout={() => cookies.clear()}
        />
      </header>
      {store.content}
    </div>
  );
}
```

동일한 username 값이 전달되더라도, MemoizedLogout은 새로운 onLogout 콜백 때문에 리렌더링을 하게 된다.  
-> 즉 메모이제이션 중단!

해결방법은 onLogout prop의 값을 매번 동일한 콜백 인스턴스로 설정하여 된다! -> 이때 useCallback 사용!

```js
const MemoizedLogout = React.memo(Logout);

function MyApp({ store, cookies }) {
  const onLogout = useCallback(() => {
    cookies.clear();
  }, []);
  return (
    <div className="main">
      <header>
        <MemoizedLogout username={store.username} onLogout={onLogout} />
      </header>
      {store.content}
    </div>
  );
}
```

### useCallback, React.memo

useCallback만 사용하면 하위컴포넌트의 리렌더링을 막을 수 없기 때문에 React.memo 를 같이 사용하고,  
React.memo만 사용하면, 부모컴포넌트가 자식 컴포넌트의 콜백 함수를 정의하면, 이로 인해 메모이제이션이 중단될수 있기때문에 useCallback 을 사용한다고 이해하였다..!! (추후에 보충사항이 있으면 업데이트 할 예정! )

### 마지막으로 React.memo 언제 쓰면 좋을까?

같은 props 자주 렌더링이 일어나는 컴포넌트다!  
⭐️ 하지만 중요한 것은 앞에서도 계속 언급이 되었지만, 사용했을 때와 사용하지 않았을 때의 성능 차이를 비교한 후, 이득이 있을 때만 사용해야 한다는 것이다!!

### 느낀점

useMemo 와 useCallback 에 대해서 간단히 정리할려고 시작한 글이, HOC, React.memo 로 마무리 되었다.  
항상 !! 매번 느끼지만,,, 공부하다보면 새로운 곳에서 또 다른 공부해야하는 것들이 생기고...또 공부하면 또 생기고...어렵다...🥲  
하지만 한편으로 재미도 있다...!! 다만, 개념을 안다고 해서 다 아는 것이 아니니...사용할 기회가 생긴다면 무조건 사용해보자!

---

https://merrily-code.tistory.com/11  
https://ui.toast.com/weekly-pick/ko_20190731  
https://crmrelease.tistory.com/41  
https://satisfactoryplace.tistory.com/163  
https://sustainable-dev.tistory.com/137  
https://www.daleseo.com/react-hooks-use-memo/

https://www.youtube.com/watch?v=XfUF9qLa3mU
https://www.youtube.com/watch?v=e-CnI8Q5RY4
