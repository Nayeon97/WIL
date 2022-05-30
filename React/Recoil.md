## 🚀 Recoil

    💡 3차 프로젝트 시작 전, Naver DEVIEW  강의를 보며 Recoil 에 대해 겉핣기로 공부해보았다!

---

작성일: 2022년 5월 29일

### Redux, modx

### Recoil : 귀한 가문에서 엘리트 교육을 받고 자란 기사

Recoil은 Redux, Mobx와 달리 리액트 **전용** 상태 관리 라이브러리로,  
상태관리를 가장 `리액트스러운` 상태관리 라이브러리다!

### recoilRoot

```js
import React from 'react';
import { RecoilRoot } from 'recoil';
import Counter from './Counter';

export default function App() {
  return (
    <RecoilRoot>
      <Counter />
    </RecoilRoot>
  );
}
```

recoil state 를 사용할 컴포넌트는 RecoilRoot 로 감싸주어야 한다.  
중첩도 가능하며, RecoilRoot 가 여러개라면, 가장 가까운 RecoilRoot를 참조한다.

### 1️⃣ Atom

하나의 데이터 상태 값(=상태 단위)이다. atom의 값이 변경될 때마다 해당 atom을 구독하는 컴포넌트들이 다시 렌더링된다!

```js
const countState = atom({
  key: 'countState', // unique한 id
  default: 0,
});
```

### useRecoilState

atom 의 state 를 `get`, `set` 할 수 있다.

```js
const [count, setCount] = useRecoilState(countState);
```

count는 countState의 value를 `get`.  
setCount는 count의 값을 `set`.

### useRecoilValue, useSetRecoilValue

```js
const count = useRecoilValue(countState);
const setCount = useSetRecoilValue(countState);
// get, set 따로 사용 가능.
```

### 2️⃣ Selector

atom에서 파생된 데이터로 다른 atom에 의존하는 동적인 데이터를 만들어줌.  
원래의 state를 가져오는 것 X -> state 가공하여 반환.

```js
const oddEvenState = selector({
  key: 'oddEvenState',
  get: ({ get }) => {
    const count = get(countState);
    return count % 2 ? '홀' : '짝';
  },
});
```

Selector 에서는 useRecoilValue를 사용하여 값을 읽을 수 있다.

```js
const oddEven = useRecoilValue(oddEvenState);
```

### Selector 을 사용한 비동기 처리.

Selector 만 사용해서 비동기 처리를 실행하면, error 가 난다.  
-> 렌더링 할 데이터가 도착하기 전에 실행이 되어서인데, React.Suspense 를 사용해서 해결할 수 있다.

```js
import { selector } from 'recoil';

export const randomCat = selector({
  key: 'randomCat',
  get: async () => {
    const response = await fetch('https://aws.random.cat/meow');
    const data = await response.json();

    return data.file;
  },
});
```

```js
import React from 'react';
import { useRecoilValue, useRecoilValueLoadable } from 'recoil';
import { randomCat } from './state';

export default function RandomCat() {
  const photoUrl = useRecoilValue(randomCat);

  return (
    <div className="random-cat">
      <img src={photoUrl} alt="random cat" />
    </div>
  );
}
```

```js
import React from 'react';
import { RecoilRoot } from 'recoil';
import RandomCat from './RandomCat';

export default function App() {
  return (
    <div className="App">
      <h1>Random Cat</h1>
      <p>페이지를 새로 고침할 때마다 랜덤한 고양이 사진을 보여줍니다!</p>
      <RecoilRoot>
        <React.Suspense fallback={null}>
          <RandomCat />
        </React.Suspense>
      </RecoilRoot>
    </div>
  );
}
```

---

#### 📎 Reference

https://deview.kr/2020/sessions/336
