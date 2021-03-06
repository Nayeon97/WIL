## ๐ Recoil

    ๐ก 3์ฐจ ํ๋ก์ ํธ ์์ ์ , Naver DEVIEW  ๊ฐ์๋ฅผ ๋ณด๋ฉฐ Recoil ์ ๋ํด ๊ฒํฃ๊ธฐ๋ก ๊ณต๋ถํด๋ณด์๋ค!

---

์์ฑ์ผ: 2022๋ 5์ 29์ผ

### Redux, modx

### Recoil : ๊ทํ ๊ฐ๋ฌธ์์ ์๋ฆฌํธ ๊ต์ก์ ๋ฐ๊ณ  ์๋ ๊ธฐ์ฌ

Recoil์ Redux, Mobx์ ๋ฌ๋ฆฌ ๋ฆฌ์กํธ **์ ์ฉ** ์ํ ๊ด๋ฆฌ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก,  
์ํ๊ด๋ฆฌ๋ฅผ ๊ฐ์ฅ `๋ฆฌ์กํธ์ค๋ฌ์ด` ์ํ๊ด๋ฆฌ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ค!

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

recoil state ๋ฅผ ์ฌ์ฉํ  ์ปดํฌ๋ํธ๋ RecoilRoot ๋ก ๊ฐ์ธ์ฃผ์ด์ผ ํ๋ค.  
์ค์ฒฉ๋ ๊ฐ๋ฅํ๋ฉฐ, RecoilRoot ๊ฐ ์ฌ๋ฌ๊ฐ๋ผ๋ฉด, ๊ฐ์ฅ ๊ฐ๊น์ด RecoilRoot๋ฅผ ์ฐธ์กฐํ๋ค.

### 1๏ธโฃ Atom

ํ๋์ ๋ฐ์ดํฐ ์ํ ๊ฐ(=์ํ ๋จ์)์ด๋ค. atom์ ๊ฐ์ด ๋ณ๊ฒฝ๋  ๋๋ง๋ค ํด๋น atom์ ๊ตฌ๋ํ๋ ์ปดํฌ๋ํธ๋ค์ด ๋ค์ ๋ ๋๋ง๋๋ค!

```js
const countState = atom({
  key: 'countState', // uniqueํ id
  default: 0,
});
```

### useRecoilState

atom ์ state ๋ฅผ `get`, `set` ํ  ์ ์๋ค.

```js
const [count, setCount] = useRecoilState(countState);
```

count๋ countState์ value๋ฅผ `get`.  
setCount๋ count์ ๊ฐ์ `set`.

### useRecoilValue, useSetRecoilValue

```js
const count = useRecoilValue(countState);
const setCount = useSetRecoilValue(countState);
// get, set ๋ฐ๋ก ์ฌ์ฉ ๊ฐ๋ฅ.
```

### 2๏ธโฃ Selector

atom์์ ํ์๋ ๋ฐ์ดํฐ๋ก ๋ค๋ฅธ atom์ ์์กดํ๋ ๋์ ์ธ ๋ฐ์ดํฐ๋ฅผ ๋ง๋ค์ด์ค.  
์๋์ state๋ฅผ ๊ฐ์ ธ์ค๋ ๊ฒ X -> state ๊ฐ๊ณตํ์ฌ ๋ฐํ.

```js
const oddEvenState = selector({
  key: 'oddEvenState',
  get: ({ get }) => {
    const count = get(countState);
    return count % 2 ? 'ํ' : '์ง';
  },
});
```

Selector ์์๋ useRecoilValue๋ฅผ ์ฌ์ฉํ์ฌ ๊ฐ์ ์ฝ์ ์ ์๋ค.

```js
const oddEven = useRecoilValue(oddEvenState);
```

### Selector ์ ์ฌ์ฉํ ๋น๋๊ธฐ ์ฒ๋ฆฌ.

Selector ๋ง ์ฌ์ฉํด์ ๋น๋๊ธฐ ์ฒ๋ฆฌ๋ฅผ ์คํํ๋ฉด, error ๊ฐ ๋๋ค.  
-> ๋ ๋๋ง ํ  ๋ฐ์ดํฐ๊ฐ ๋์ฐฉํ๊ธฐ ์ ์ ์คํ์ด ๋์ด์์ธ๋ฐ, React.Suspense ๋ฅผ ์ฌ์ฉํด์ ํด๊ฒฐํ  ์ ์๋ค.

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
      <p>ํ์ด์ง๋ฅผ ์๋ก ๊ณ ์นจํ  ๋๋ง๋ค ๋๋คํ ๊ณ ์์ด ์ฌ์ง์ ๋ณด์ฌ์ค๋๋ค!</p>
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

#### ๐ Reference

https://deview.kr/2020/sessions/336
