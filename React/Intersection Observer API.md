## ๐ Scroll ์ด๋ฒคํธ ๋์  Intersection Observer API ์ด์ฉํ์! 

     3์ฐจ ํ๋ก์ ํธ ๋ฆฌํฉํ ๋ง! 
---

์์ฑ์ผ: 2022๋ 7์ 23์ผ

### ๐ฑ Scroll ์ด๋ฒคํธ๋ฅผ ํ์ฉํ ๋ฌดํ ์คํฌ๋กค! 

์ ์ฒด ์ผ๊ธฐ ๋ณด์ฌ์ฃผ๋ ํ์ด์ง๋ฅผ ์ปค์ ๊ธฐ๋ฐ ๋ฌดํ ์คํฌ๋กค๋ก ๊ตฌํํ์๋๋ฐ, ์ด๋ window ๊ฐ์ฒด์ scroll ์ด๋ฒคํธ๋ฅผ ์ฐ๊ฒฐํ์ฌ ํน์  ์ง์ ์ ์คํฌ๋กค์ด ๋์์ ๋, ์๋ฒ์ ๋ค์ ์ผ๊ธฐ ๋ฆฌ์คํธ๋ฅผ ์์ฒญํ๊ณ  ์ด๋ ํ๋ผ๋ฏธํฐ์ cursor ๊ฐ์ ๋ณด๋ด๋ ํ์์ผ๋ก ํ์๋ค!    

```js
// handleScroll.js
export const handleScroll = () => {
  const scrollHeight = document.documentElement.scrollHeight;
  const scrollTop = document.documentElement.scrollTop;
  const clientHeight = document.documentElement.clientHeight;
  if (scrollTop + clientHeight >= scrollHeight) {
    return true; // true ๊ฐ ๋ฐํ
  }
};
```

```js
//  EmotionList.js
  ...
  const [isLoaded, setIsLoaded] = useState(true); 
  const [stop, setStop] = useState(false);   
  useEffect(() => {
    if (isLoaded && !stop) { 
      getList();
    }
  }, [isLoaded]);

  useEffect(() => {
    window.addEventListener(
      'scroll',
      function (event) {
        const res = handleScroll(event);
        if (res === true) { // true ๊ฐ์ด ๋ฐํ๋  ๋ 
          setIsLoaded(true); // setIsLoaded(true)๋ก ์ํ ๋ณ๊ฒฝ
        }
      },
      false
    );
  }, []);

    const getList = async () => {
    if (isLoaded === true) {
      try {
        const res = await Api.get(`diary/list/?cursor=${cursor}`);
        const length = res.data.length;
        const sliceData = res.data.slice(0, length - 1);
        setCursor(res.data.slice(-1)[0].cursor);
        setDiaryList((data) => [...data, ...sliceData]);
        setIsLoaded(false);
        if (length < 10) {
          setStop(true);
        }
      } catch (err) {
        snackBar('info', '๋ ์ด์ ์์ฑํ ์ผ๊ธฐ๊ฐ ์์ต๋๋ค. ');
      }
    }
  };
```
### ๐ฅ ๋ฌธ์  
1. ์์ฒญํ  ์ผ๊ธฐ ๋ฆฌ์คํธ๋ ๋ ์ด์ ์์ง๋ง, ์คํฌ๋กค๋ก ์ธํด ๋ถํ์ํ ์์ฒญ ๋ฐ์.
2. ์คํฌ๋กค ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ๋ฉด ์ด๋ฒคํธ๊ฐ ํธ์ถ๋์ด ๋ฉ์ธ ์ค๋ ๋ ์ฑ๋ฅ์ ์ข์ง ์์.

![image](https://user-images.githubusercontent.com/89979344/180594160-9010a01e-fb4a-4678-846b-d1d4cae8cda0.png)


### ๐ก ๋ฌธ์  1 ํด๊ฒฐ
#### stop ์ด๋ผ๋ ์ํ๊ฐ ์ค์  
์ฌ์ฉ์๊ฐ ๋ฐ์์จ ์ผ๊ธฐ ๋ฆฌ์คํธ๊ฐ 10๊ฐ ๋ฏธ๋ง์ด๋ผ๋ฉด(์ผ๊ธฐ ๋ฆฌ์คํธ 10๊ฐ์ฉ ๋ฐ์์ด.)   
๋ ์ด์ ์ผ๊ธฐ ๋ฆฌ์คํธ๊ฐ ์๋ค๋ ๋ป์ด๊ธฐ ๋๋ฌธ์ stop ์ํ๊ฐ์ `true`๋ก ๋ณ๊ฒฝ!   
์คํฌ๋กค ์ด๋ฒคํธ๊ฐ ๋ฐ์ํ์ฌ๋, stop ๊ฐ์ด true ์ด๊ธฐ๋๋ฌธ์ getList ํจ์ ํธ์ถ X.
```js
 const [stop, setStop] = useState(false); 
  
  useEffect(() => {
    if (isLoaded && !stop) {  // stop ๊ฐ์ด false ์ผ๋๋ง ํธ์ถ.
      getList();
    }
  }, [isLoaded]);

  useEffect(() => {
    window.addEventListener(
      'scroll',
      function (event) {
        const res = handleScroll(event);
        if (res === true) { // true ๊ฐ์ด ๋ฐํ๋  ๋ 
          setIsLoaded(true); // setIsLoaded(true)๋ก ์ํ ๋ณ๊ฒฝ
        }
      },
      false
    );
  }, []);

    const getList = async () => {
    if (isLoaded === true) {
      try {
        const res = await Api.get(`diary/list/?cursor=${cursor}`);
        const length = res.data.length;
        const sliceData = res.data.slice(0, length - 1);
        setCursor(res.data.slice(-1)[0].cursor);
        setDiaryList((data) => [...data, ...sliceData]);
        setIsLoaded(false);
        if (length < 10) {
          setStop(true);
        } 
     // ๋ง์ฝ ์๋ฒ์์ ์ ๋ฌ๋ฐ์ ์ผ๊ธฐ๋ฆฌ์คํธ๊ฐ 10๊ฐ ๋ฏธ๋ง์ด๋ผ๋ฉด
     // ์ผ๊ธฐ ๋ฆฌ์คํธ๊ฐ ๋ ์ด์์๋ค๋ ๋ป -> stop ์ํ๊ฐ true ๋ก ๋ณ๊ฒฝ. 
      } catch (err) {
        snackBar('info', '๋ ์ด์ ์์ฑํ ์ผ๊ธฐ๊ฐ ์์ต๋๋ค. ');
      }
    }
  };
```

๋ฌธ์  1์ด ์๋ฒฝํ๊ฒ ํด๊ฒฐํ์ง ๋ชปํ๋ค. 
์๋ฅผ ๋ค์ด ์ฌ์ฉ์์ ์ผ๊ธฐ๊ฐ 30๊ฐ๋ผ๋ฉด? 10 -> 10 -> 10  
๊ธธ์ด๋ 10๊ฐ ์ด๊ธฐ๋๋ฌธ์ stop ์ํ๊ฐ์ด ๋ณ๊ฒฝ๋์ง ์๋๋ค. 

### ๐ค ๋ฌธ์  2 ํด๊ฒฐ ๋ฐฉ๋ฒ์ ?     
      
๊ณ ๋ฏผ ๋์ `Throttle` ์ ์ฌ์ฉํ์ฌ ์ต์ ํ ์์์ ํ๋ ค๊ณ  ํ๋ค.      

#### ๐ Throttle ?   

์ด๋ฒคํธ์ ์ํ ์ฝ๋ฐฑ์ ์ผ์  ์๊ฐ ๋ค์ ํธ์ถ.     
์ด๋ฐ ์ฑ์ง์ ์ด์ฉํด์, scroll ์ด๋ฒคํธ์ ์ ์ฉํ์ ๋ ์ค์ ํ ์๊ฐ๋์์ ์คํฌ๋กค์ ํ๋๋ผ๋ 
์ฝ๋ฐฑ์ด ํธ์ถ๋์ง ์๋๋ค.        

### โญ๏ธ Intersection Observer API โญ๏ธ  

Throttel ์ ๋ํด ์์๋ณด๋ ์ค Intersection Observer API ์ ์๊ฒ ๋์๋ค!

โ๏ธ ๊ต์ฐจ ๊ด์ฐฐ์ API(Intersection Observer API)๋ ์์์์ ๋๋ ์ต์์ ๋ฌธ์์ ๋ทฐํฌํธ  ์      
๋์ ์์์ ๊ต์ฐจ์ ์์ ๋ณํ๋ฅผ ๋น๋๊ธฐ์ ์ผ๋ก ๊ด์ฐฐํ  ์ ์๋ ๋ฐฉ๋ฒ์ ์ ๊ณตํ๋ค.  

์ฝ๊ฒ ๋งํด, scroll ์ด๋ฒคํธ์ ๋ฌ๋ฆฌ ํ๊ฒ ์์๊ฐ ๋ค๋ฅธ ์์์ ๋ค์ด๊ฐ๊ฑฐ๋ ๋๊ฐ ๋ or      
์ง์ ํ ๋งํผ ๋ ์์์ ๊ต์ฐจ ๋ถ๋ถ์ด ๋ณ๊ฒฝ๋ ๋๋ง๋ค ๋ฑ๋กํ ์ฝ๋ฐฑํจ์๋ฅผ ์คํํ  ์ ์๋ค!   
=> ๋ฌธ์  2 ํด๊ฒฐ!    

### โ๏ธ ์์ ํ ์ฝ๋
```js
  useEffect(() => {
    let observer;
    if (target) {
      observer = new IntersectionObserver(getList, {
        threshold: 0.7,
      });
      observer.observe(target);
    }
    return () => observer && observer.disconnect();
  }, [target]);

  const getList = async ([entry]) => {
    if (entry.isIntersecting && !stop) {
      try {
        setLoading(true);
        const res = await Api.get(`diary/list/?cursor=${cursor}`);
        if (res.data.length === 0) {
          setStop(true);
          setLoading(false);
        } else {
          const length = res.data.length;
          const sliceData = res.data.slice(0, length - 1);
          setCursor(res.data.slice(-1)[0].cursor);
          setDiaryList((data) => [...data, ...sliceData]);
          setLoading(false);
        }
      } catch (err) {
        snackBar('info', '๋ ์ด์ ์์ฑํ ์ผ๊ธฐ๊ฐ ์์ต๋๋ค. ');
      }
    }
  };
```

---
### ๐ reference 

https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API
