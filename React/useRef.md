## ๐ useRef

---

์์ฑ์ผ: 2022๋ 5์ 17์ผ

### 1๏ธโฃ useRef ๋?

```js
const refContainer = useRef(initialValue);
```

useRef๋ `.current` ํ๋กํผํฐ๋ก ์ ๋ฌ๋ ์ธ์(initialValue) ๋ก ์ด๊ธฐํ๋ ๋ณ๊ฒฝ ๊ฐ๋ฅํ ref ๊ฐ์ฒด๋ฅผ ๋ฐํ,  
๋ฐํ๋ ๊ฐ์ฒด๋ ์ปดํฌ๋ํธ์ ์  ์์ ์ฃผ๊ธฐ๋ฅผ ํตํด ์ ์ง๋๋ค๊ณ  ํ๋ค.

<img src="../imgs/useRef.png" width="600" height="300"/>

### 2๏ธโฃ useRef ๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ!

1.  ํน์  DOM ์ ํํ๊ธฐ.
2.  ์ปดํฌ๋ํธ ์์ ๋ณ์ ๋ง๋ค๊ธฐ.

### โญ๏ธ ํน์  DOM ์ ํํ๊ธฐ.

useRef ๋ฅผ ์ด์ฉํ์ฌ ํน์  DOM ์ ์ ํํ  ์ ์๋๋ฐ, ๋๋ input ์ ์๋ ฅํ์ง ์์ ์ํ์์ ์ ์ก๋ฒํผ์ ๋๋ฅด๋ฉด input ์ด ํฌ์ปค์ค๋๋ ๊ฒ์ useRef ๋ฅผ ์ด์ฉํ์ฌ ๊ตฌํํด๋ณธ ์ ์ด ์๋ค!

```js
import { useRef, useState } from 'react';

const Memo = () => {
  const memoInput = useRef();
  const [memo, setMemo] = useState('');

  const handleChangeState = (e) => {
    setMemo(e.target.value);
  };

  const handleSubmit = () => {
    if (memo.length < 1) {
      memoInput.current.focus();
      return;
    }

    console.log(state);
    alert('์ ์ฅ ์ฑ๊ณต!');
  };

  return (
    <div>
        <input
          ref={memoInput}
          value={memo}
          onChange={handleChangeState}
          type="text"
        />
      </div>
      <div>
        <button onClick={handleSubmit}>๋ฉ๋ชจ ์ ์ฅํ๊ธฐ</button>
      </div>
  );
};
export default Memo;
```

DOM ์์๋ฅผ ์ ํํ๋ ๋ถ๋ถ์ ์ด์ ๋๋ก๋ง ํ๊ณ  ๋์ด๊ฐ๊ฒ ๋ค!

### โญ๏ธ ์ปดํฌ๋ํธ ์์ ๋ณ์ ๊ด๋ฆฌ.

useRef๋ก ๋ณ์๋ฅผ ๊ด๋ฆฌํ๊ฒ ๋๋ฉด, ๊ทธ ๋ณ์๊ฐ ์๋ฐ์ดํธ ๋๋ค๊ณ  ํด์ ์ปดํฌ๋ํธ๊ฐ ๋ฆฌ๋ ๋๋ง ๋์ง ์๋๋ค๊ณ  ํ๋ค! ๐ฎ  
๋ค๋ง, ๋ฆฌ์กํธ ์ปดํฌ๋ํธ์์์ ์ํ๋ ์ํ๋ฅผ ๋ฐ๊พธ๋ ํจ์๋ฅผ ํธ์ถํ๊ณ  ๋์ ๊ทธ ๋ค์ ๋ ๋๋ง ์ดํ๋ก ์๋ฐ์ดํธ ๋ ์ํ๋ฅผ ์กฐํ ํ  ์ ์์ง๋ง,  
useRef ๋ก ๊ด๋ฆฌํ๊ณ  ์๋ ๋ณ์๋ ์ค์  ํ์ ๋ฐ๋ก ์กฐํ๊ฐ ๊ฐ๋ฅํ๋ค๊ณ  ํ๋ค!  
๋ฆฌ๋ ๋๋ง์ ํ  ํ์๊ฐ ์๋ ๋ณ์๋ผ๋ฉด! useRef ๋ก ๊ด๋ฆฌํด์ฃผ์!

```js
import React, { useRef } from 'react';

export default function App() {
  const countRef = useRef(0);

  const plusCount = () => {
    countRef.current += 1;
  };

  return (
    <div className="App">
      <h1>{countRef.current}</h1>
      <button onClick={plusCount}>1 ๋ํ๊ธฐ</button>
      <button
        onClick={() => {
          console.log(countRef.current);
        }}
      >
        ์ฝ์
      </button>
    </div>
  );
}
```

์์ ์ฝ๋๋ฅผ ์คํํด๋ณด๋ฉด, ํ๋ฉด์ ๋ณด์ด๋ ์ซ์๋ 0์์ ๋ฐ๋์ง ์์ง๋ง, console.log ๋ฅผ ์ฐ์ด๋ณด๋ฉด ๊ฐ์ด ์ฆ๊ฐํ๊ณ  ์์์ ์ ์ ์๋ค!  
-> ๊ทธ๋ฆฌ๊ณ  ์ด ๊ฐ์ ์ธ๋ง์ดํธ ๋  ๋๊น์ง ์ ์ง!

๋ฆฌ๋ ๋๋ง ํ์์๋ ๋ณ์๋ฅผ ์ฌ์ฉํ  ๋ ํ๋ฒ ์ฌ์ฉํด๋ณด์!

---

https://ko.reactjs.org/docs/hooks-reference.html#useref
https://hyeok999.github.io/2020/01/07/react-velo-10/
https://www.youtube.com/watch?v=VxqZrL4FLz8
