## ๐ useContext ๋ฅผ ๋ง์ด ์ฐ๋ฉด ์๋๋ค...?

---

์์ฑ์ผ: 2022๋ 4์ 12์ผ

        ๐ค ์ฝํ ์คํฐ๋๊ฐ ๋๋๊ณ , ์ ์ ๋ฆฌ์กํธ์ ๋ํด ์ด์ผ๊ธฐ๋ฅผ ๋๋๋ ์ค์, useContext ์ ๋ํด ์คํฐ๋์ ํ๋ถ์ด useContext ๋ฅผ ๋ง์ด ์ฐ๋ฉด ์ข์ง์๋ค๋
        ์ด์ผ๊ธฐ๋ฅผ ๋ค์ผ์จ๋ค๊ณ  ํ๋ค. useContext ๋ฅผ ์ฌ์ฉํ๋ฉด, ๋จ๋ฐํ๋ Props ์ ๋ฌ์ ์๋ฐฉํ  ์ ์๋๋ฐ...
         ์?????? ๋ผ๋ ์๊ฐ์ ์ด๋ฒ WIL ์ ์ฃผ์ ๋ useContext ์ด๋ค.

### 1๏ธโฃ useContext ๋?

**<u>context๋ฅผ ์ด์ฉํ๋ฉด ๋จ๊ณ๋ง๋ค ์ผ์ผ์ด props๋ฅผ ๋๊ฒจ์ฃผ์ง ์๊ณ ๋ ์ปดํฌ๋ํธ ํธ๋ฆฌ ์ ์ฒด์ ๋ฐ์ดํฐ๋ฅผ ์ ๊ณตํ  ์ ์์ต๋๋ค.</u>**

> ๐ก ์ผ๋ฐ์ ์ธ React ์ ํ๋ฆฌ์ผ์ด์์์ ๋ฐ์ดํฐ๋ ์์์ ์๋๋ก (์ฆ, ๋ถ๋ชจ๋ก๋ถํฐ ์์์๊ฒ) props๋ฅผ ํตํด ์ ๋ฌ๋์ง๋ง, ์ ํ๋ฆฌ์ผ์ด์ ์์ ์ฌ๋ฌ ์ปดํฌ๋ํธ๋ค์ ์ ํด์ค์ผ ํ๋ props์ ๊ฒฝ์ฐ (์๋ฅผ ๋ค๋ฉด ์ ํธ ๋ก์ผ์ผ, UI ํ๋ง) ์ด ๊ณผ์ ์ด ๋ฒ๊ฑฐ๋ก์ธ ์ ์์ต๋๋ค. context๋ฅผ ์ด์ฉํ๋ฉด, ํธ๋ฆฌ ๋จ๊ณ๋ง๋ค ๋ช์์ ์ผ๋ก props๋ฅผ ๋๊ฒจ์ฃผ์ง ์์๋ ๋ง์ ์ปดํฌ๋ํธ๊ฐ ์ด๋ฌํ ๊ฐ์ ๊ณต์ ํ๋๋ก ํ  ์ ์์ต๋๋ค.

๋ฆฌ์กํธ ๊ณต์๋ฌธ์ useContext์ ๊ดํด ๊ธฐ์ฌ๋์ด์๋ ์ค๋ช์ด๋ค.

๊ณต์ ๋ฌธ์์ ๊ธฐ์ฌ๋ ์์ ์ค๋ช๋ง์ผ๋ก useContext ์ ๋ํ ์ดํด๊ฐ ์ ๋์๋ค.( ์ธ์  ๊ฐ ํ๋ก์ ํธ ๊ธฐ๊ฐ ์ฝ์น๋๊ป ๋ฆฌ์กํธ ๊ด๋ จ๋ ์ฑ ์ถ์ฒ์ ์ฌ์ญค๋ณด์๋๋ฐ ์ฑ๋ณด๋ค ๊ณต์๋ฌธ์! ๋ผ๋ ๋ต๋ณ์ ์ฃผ์จ๋๋ฐ, ๊ทธ ์ด์ ๊ฐ ์ดํด๊ฐ ๋์๋ค. )<br />
์๋ฅผ ๋ค์ด์ ๋ค์ useContext ์ ๋ํด ๋งํด๋ณด์๋ฉด...์....๐ค (tmi...์์ฆ ๊ธ์ ์ธ๋, ์ต๋ํ ๋์ ์ธ์ด๋ก ํ์ดํด์ ์ฐ๊ณ ์ ๋ธ๋ ฅํ๋ค....์์ง ์ต์ํ์ง ์์์ ์ ๋์ง๋ ์์ง๋ง.. ๊ณ์ํด์ ๊ณ ๋ฏผํ๋ค ๋ณด๋ฉด, ๋์ง์์๊น....?)
์๋ฌดํผ ๋ค์ ๋์์์ ์๋ฅผ ๋ค์ด ๋ด๊ฐ ๋ํ๋ณ์์ 5์ธต์ ์๋ ์ธ๊ณผ A์ ์๋์๊ฒ ์ง๋ฃ๋ฅผ ๋ณด์์ผํ๋ค๋ฉด, ์ผ๋จ 1์ธต์ ์๋ ์ ์์ฒ์ ์ ์๋ฅผ ํ  ๊ฒ์ด๋ค. <br />
๊ทธ๋ฌ๊ณ  ๋์ 2์ธต, 3์ธต, 4์ธต, 5์ธต์ ๊ฐ ์ธต์ ์ ์์ฒ๊ฐ ์๋ค๊ณ  ๊ฐ์ ์ ํ๊ณ , ๊ทธ๋ผ ๋ด๊ฐ 5์ธต์ ์๋ ์ธ๊ณผ A ์ ์๋์ ๋ง๋๊ธฐ ์ํด 2์ธต์๋ ์ ์ -> 3์ธต์๋ ์ ์ -> 4์ธต์๋ ์ ์ -> 5์ธต์๋ ์ ์ ํ์ ์ ์๋์ ๋ง๋์ง ์๋๋ค!!!! ๊ทธ์  1์ธต์ ํ๋ฒ ๋ฑ๋ก์ ํ๊ณ  5์ธต์ผ๋ก ๊ฐ์ ๋๋ฅผ ๋ถ๋ฅด๋ฉด ๋๋ ์ง๋ฃ์ค์ ๋ค์ด๊ฐ ๊ฒ์ด๋ค!

useContext ๋ ๋๊ฐ๋ค. ๋ฐ์ดํฐ๋ฅผ ์ค๊ฐ ์๋ฆฌ๋จผํธ์ Props๋ฅผ ์ด์ฉํ์ฌ ๊ณ์ ๋๊ฒจ์ค ํ์๊ฐ ์์ด ํด๋น ๋ฐ์ดํฐ๊ฐ ํ์ํ ์ปดํฌ๋ํธ๋ก ์๊ฐ์ด๋ ์์ผ์ค ์ ์๋ค!

### 2๏ธโฃ useContext ์ฌ์ฉ๋ฐฉ๋ฒ ?

โ createContext : context ๊ฐ์ฒด๋ฅผ ์์ฑ.

```js
const { Provider, Consumer } = React.createContext(defaultValue)
```

โ๏ธ defaultValue : Context ๋ฅผ ์ฌ์ฉํ๋ ์ปดํฌ๋ํธ์ ์์์ Provider๊ฐ ์๋ ๊ฒฝ์ฐ์ ์ฌ์ฉ๋๋ ๊ฐ.  
 ๐ Provider value๋ก `undefined` ๋ฅผ ์ ๋ฌํ๋ฉด, Consumer ๊ฐ defaultValue ๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ ๋ง๋๋ค.

โ Provider : ์์ฑํ context๋ฅผ ํ์ ์ปดํฌ๋ํธ์๊ฒ ์ ๋ฌํ๋ ์ญํ .  
 โ๏ธ ํ๋์ Provider ์ ์ฌ๋ฌ๊ฐ์ Consumer ๋ค์ด ์ฐ๊ฒฐ ๋  ์ ์๋ค.

โ Consumer : context์ ๋ณํ๋ฅผ ๊ฐ์ํ๋ ์ปดํฌ๋ํธ.  
 โ๏ธ context ๋ณํ๋ฅผ ๊ฐ์งํ๊ธฐ ์ํ ๋ฆฌ์กํธ ์ปดํฌ๋ํธ.

โ ์์

1. ์ต์๋จ ์ปดํฌ๋ํธ์์ createContext๋ฅผ import.
2. 'UserContext'๋ผ๋ ๋ณ์์ ๋ด์ ๋ด๋ณด๋ผ ์ค๋น๋ฅผ ํ๋ค.
3. ์ฌ์ฉํ๊ณ ์ ํ๋ state๋ฅผ value๊ฐ์ ๋ด์ ๋ณ์๋ก ์ง์ ํ๋ค.
4. ํ์ ์ปดํฌ๋ํธ๋ค์ UserContext.Provider๋ก ๊ฐ์ธ๊ณ , value๊ฐ์ ๋ณด๋ด์ค๋ค.

```js
import React, { createContext } from 'react'
import Children from './Children'

export const UserContext = createContext(defaultValue)
// defaultValue : Context ๋ฅผ ์ฌ์ฉํ๋ ์ปดํฌ๋ํธ์ ์์์ Provider๊ฐ ์๋ ๊ฒฝ์ฐ์ ์ฌ์ฉ๋๋ ๊ฐ.

const App = () => {
  const user = {
    userId: 'abcd',
    userName: '๋์ฐ',
  }

  return (
    <>
      <UserContext.Provider value={user}>
        <div>
          <User />
        </div>
      </UserContext.Provider>
    </>
  )
}

export default App
```

๋ฐ์ดํฐ๋ฅผ ์ฌ์ฉํด์ผํ๋ ํด๋น ์ปดํฌ๋ํธ์์ useContext ๋ฅผ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ์๋ ๋๊ฐ์ง๊ฐ ์๋ค.

โ ์ฒซ๋ฒ์งธ ๋ฐฉ๋ฒ

```js
import React from 'react'
import { UserContext } from './App'

const User = () => {
  return (
    <UserContext.Consumer>
      {(user) => (
        <>
          <h2>userId๋ {user.userId}์๋๋ค.</h2>
          <h2>userName {user.userName}์๋๋ค.</h2>
        </>
      )}
    </UserContext.Consumer>
  )
}

export default User
```

โ ๋๋ฒ์งธ ๋ฐฉ๋ฒ

```js
import React from 'react'
import { userContext } from './App'
import { UserContext } from 'react'

const User = () => {
  const { userId, userName } = useContext(UserContext)

  return (
    <>
      <h2>userId :{userId}</h2>
      <h2>userName :{userName}</h2>
    </>
  )
}

export default User
```

์ด ๋ ๋ฐฉ๋ฒ์ ์ฐจ์ด์ ์ ์์ง ์ฐพ์ง๋ ๋ชปํ๋๋ฐ, ๋ด ์๊ฐ์๋ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ ๋ฐ๋ก ์ถ๋ ฅ๋ง ํ๋ค๋ฉด consumer ์ ์ฌ์ฉํ๋ ๊ฒ์ด ํธํ  ๊ฒ ๊ฐ๊ณ , ๋ฐ์ดํฐ๋ฅผ ์ด์ฉํด์ผํ๋ค๋ฉด ๋๋ฒ์งธ ๋ฐฉ๋ฒ์ด ํธํ  ๊ฒ๊ฐ๋ค. ๋์ ๊ฒฝ์ฐ๋ค๋ ๋๋ฒ์งธ ๋ฐฉ๋ฒ์ด ์กฐ๊ธ ๋ ํธํด์ ์ฌ์ฉ์ค์ด๋ค..๋ค๋ง, ์ถ๋ ฅ๋ง ํด์ผํ๋ ๊ฒฝ์ฐ๊ฐ ์๊ธด๋ค๋ฉด ์ฒซ๋ฒ์งธ ๋ฐฉ๋ฒ์ ์ฌ์ฉํ  ๊ฒ๊ฐ๋ค!  
์ด๋ถ๋ถ์ ์กฐ๊ธ ๋ ๋ณด์ถฉํด์ผ ํ  ๊ฒ ๊ฐ๋ค!

### 3๏ธโฃ context๋ฅผ ์ฌ์ฉํ๊ธฐ ์ ์ ๊ณ ๋ คํ  ๊ฒ

> context์ ์ฃผ๋ ์ฉ๋๋ ๋ค์ํ ๋ ๋ฒจ์ ๋ค์คํ๋ ๋ง์ ์ปดํฌ๋ํธ์๊ฒ ๋ฐ์ดํฐ๋ฅผ ์ ๋ฌํ๋ ๊ฒ์๋๋ค.  
>  context๋ฅผ ์ฌ์ฉํ๋ฉด ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฌ์ฉํ๊ธฐ๊ฐ ์ด๋ ค์์ง๋ฏ๋ก ๊ผญ ํ์ํ  ๋๋ง ์ฐ์ธ์.  
>  ์ฌ๋ฌ ๋ ๋ฒจ์ ๊ฑธ์ณ props ๋๊ธฐ๋ ๊ฑธ ๋์ฒดํ๋ ๋ฐ์ context๋ณด๋ค ์ปดํฌ๋ํธ ํฉ์ฑ์ด ๋ ๊ฐ๋จํ ํด๊ฒฐ์ฑ์ผ ์๋ ์์ต๋๋ค.

๊ณต์๋ฌธ์์ ๊ธฐ์ฌ๋ ๋ด์ฉ์ด๋ค.  
์...? ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฌ์ฉํ๊ธฐ๊ฐ ์ด๋ ค์์ง๊น....? ์ด ๋ถ๋ถ์ ์ดํดํด๋ณด์.

### 4๏ธโฃ useContext์์ ์ปดํฌ๋ํธ ์ฌ์ฌ์ฉ์ด ์ด๋ ต๋ค...?

<img src="../imgs/useContext2.png" width="600" height="200"/>

์ด ๋ถ๋ถ๊ณผ ๊ด๋ จํด์ ์คํฐ๋ ํ์๋ถ์ด ์ฌ๋ ค์ฃผ์  ์ค๋ช์ด๋ค...!!  
์ด ๋ถ๋ถ์ ์ฌ์ค ์ดํด๊ฐ ์๋ฒฝํ๊ฒ ๋์ง ์๋๊ฒ๊ฐ๋ค ใ ใ  ์ฝ๊ฐ ..์ ๊ทธ๋ ๊ตฌ๋...  
๊ทธ๋ฅ ๋ฌด์กฐ๊ฑด Provider ์์ ๊ฐ์ธ์ ธ ์์ด์ผ..value ๋ฅผ ๋ฐ์ ์ ์์ผ๋๊น.. ๋ถ๋ฆฌ๊ฐ ์๋๋ ๊ตฌ๋ -> ๊ทธ๋์ ์ฌ์ฌ์ฉ์ฑ์ด ๋จ์ด์ง๋ค๋๊ตฌ๋! ์ด์ ๋์ ๋๋์ด๋ค...  
์กฐ๊ธ ๋ ๊ณ ๋ฏผํ๊ณ  ์๋ฃ๋ฅผ ์ฐพ์๋ด์ผ ํ  ๊ฒ ๊ฐ๋ค. (์ฌ์ค ์ด ๋ถ๋ถ์ ์๋ฒฝํ ํ ์ํ์์ ๊ธ์ ์ฐ๊ณ  ์ถ์ด์, ๊ณ์ ๊ธ์ ์์ฑํ๋ ๊ฒ์ ๋ฏธ๋ฃจ๊ฒ ๋์๋๋ฐ, ๋ง๋ฅ ๋ฏธ๋ฃฐ์๋ ์์ด์ ์ด๋ ๊ฒ ์ ์ด๋๊ณ , ์์ ํ  ์์ ์ด๋ค!)

### 5๏ธโฃ ์ฃผ์์ฌํญ! ๋ชจ๋  ํ์ ์ปดํฌ๋ํธ๊ฐ ๋ ๋๋ง๋๋ค...!!

**useContext ์ ๋จ์ ์ผ๋ก Provider์ ๋ถ๋ชจ๊ฐ ๋ ๋๋ง ๋  ๋๋ง๋ค ๋ถํ์ํ๊ฒ ํ์ ์ปดํฌ๋ํธ๊ฐ ๋ค์ ๋ ๋๋ง ๋๋ ๋ฌธ์ ๊ฐ ์๊ธธ ์ ์๋ค!!๐ฌ ํ์ง๋ง ์ญ์๋.. ๋ฌธ์  ํด๊ฒฐ๋ฐฉ๋ฒ์ ์กด์ฌํ์๋ค!**

โ ํด๊ฒฐ์ฑ : useMemo ์ฌ์ฉํ๊ธฐ.

```js
function CountProvider(props) {
  const [count, setCount] = React.useState(0)
  const value = React.useMemo(() => [state, setCount], [count])
  return <CountContext.Provider value={value} {...props} />
}
```

[ํด๋น ์๋ฃ](https://github.com/kentcdodds/old-kentcdodds.com/blob/319db97260078ea4c263e75166f05e2cea21ccd1/content/blog/how-to-optimize-your-context-value/index.md)์์ useMemo๋ฅผ ์ฌ์ฉํ๊ฒ ๋๋ฉด, state ๊ฐ์ด ๋ณ๊ฒฝ๋  ๋๋ง, ๋ ๋๋ง๋๋๋ก ํ  ์ ์๊ธฐ๋๋ฌธ์, ๋ถํ์ํ ๋ ๋๋ง์ ๋ฐฉ์งํ  ์ ์๋ค๊ณ  ํ๋ค. ํ์ง๋ง ๋๋ ์ด๊ฒ ์ต์ ์ ์๋ ๊ฒ ๊ฐ๋ค๋ ์๊ฐ์ด ๋ค์๋ค!

```js
function CountProvider(props) {
  const [count, setCount] = React.useState(0)
  const value = React.useMemo(() => [state, setCount], [count])
  return
  ;<CountContext.Provider value={value} {...props}>
    <useCountContextComponent />
    <notUseCountContextComponent />
  </CountContext.Provider>
}
```

์๋ฅผ ๋ค์ด ์์์ useCountContextComponent ๋ CountContext.Provider์ ๋๊ฒจ์ค value ๋ฅผ ์ฌ์ฉํ์ง๋ง,notUseCountContextComponent ๋ ์ฌ์ฉํ์ง ์๋ ์ปดํฌ๋ํธ๋ผ๊ณ  ํ๋ฉด, ๊ฒฐ๊ตญ value ๊ฐ์ด ๋ฐ๋ ๋ value ๋ฅผ ์ฌ์ฉํ์ง ์๋ notUseCountContextComponent ์ปดํฌ๋ํธ๋ ๋ ๋๋ง ๋๊ธฐ ๋๋ฌธ์, ๊ฐ์ด ๋ฐ๋์ง ์์ ๋๋ ๋ค์ ๋ ๋๋ง์ด ๋๋ ๋ถ๋ถ์ useMemo ๋ก ํด๊ฒฐํ  ์ ์์ง๋ง, ์๋ฒฝํ๊ฒ ๋ชจ๋  ์ํฉ์ ํด๊ฒฐํ  ์ ๋ ์๋ค๋ ์๊ฐ์ด ๋ค์๋ค...๊ทธ๋ ๊ธฐ์ useMemo ๊ฐ ํ์คํ ํด๊ฒฐ์ฑ์ ์๋๊ฒ ๊ฐ๋ค๋ ์๋ฌธ์ด ์๊ฒผ๋ค..

ํด๋น ๋ถ๋ถ์ ๋ํด ๋ ์ฐพ๋ค๋ณด๋ ์์ธํ ์ค๋ช๋์ด์๋ [๋ธ๋ก๊ทธ](https://velog.io/@yrnana/Context-API%EA%B0%80-%EC%A1%B4%EC%9E%AC%ED%95%98%EC%A7%80%EB%A7%8C-%EC%97%AC%EC%A0%84%ED%9E%88-%EC%82%AC%EB%9E%8C%EB%93%A4%EC%9D%B4-redux%EC%99%80-%EC%A0%84%EC%97%AD-%EC%83%81%ED%83%9C%EA%B4%80%EB%A6%AC-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC%EB%A5%BC-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0#context-api%EC%9D%98-%EB%AC%B8%EC%A0%9C) ๋ฅผ ๋ฐ๊ฒฌํ์๋๋ฐ, ์ฌ๊ธฐ์ ์ปจํ์คํธ๋ฅผ ์ํ๊ฐ / ์ก์์ผ๋ก ๋๋๋ ๋ฐฉ๋ฒ์ด ์์๋ค! ๋ค๋ง, ๋ชจ๋  ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ  ์ ์๊ณ , ๊ทธ๋ ๊ธฐ ๋๋ฌธ์ ๋ง์ ๊ฐ๋ฐ์๋ค์ด ๋ฆฌ๋์ค๋ฅผ ์ฌ์ฉํ๊ณ  ์๋ค๊ณ  ํ๋ค..!!

์ด๋ก์จ ๋์ ๊ถ๊ธ์ฆ์ด ํด๊ฒฐ๋ ๊ฒ ๊ฐ๋ค!! (๋ฌผ๋ก , ์ฌ์ฌ์ฉ์ฑ์ ๋ํ ๋ถ๋ถ์ ๋ ๊ณต๋ถํด์ผํ  ๋ถ๋ถ์ด๋ค.)

๋ฌผ๋ก  ์์ง ๋ง์ ๋ถ๋ถ์์ ๋ถ์กฑํ๊ธฐ์ ์ง๊ธ์ ๊ธ์ ๊ณ์ํด์ ์กฐ๊ธ์ฉ ๋ณด์ํด๋๊ฐ ์์ ์ด๋ค!!

---

https://dongmin-jang.medium.com/reactjs-context-api-korean-%ED%95%9C%EA%B8%80-%EC%9E%91%EC%84%B1%EC%A4%91-79edaf18efff
