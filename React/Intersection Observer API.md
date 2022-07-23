## 🚀 Scroll 이벤트 대신 Intersection Observer API 이용하자! 

     3차 프로젝트 리팩토링! 
---

작성일: 2022년 7월 23일

### 🖱 Scroll 이벤트를 활용한 무한 스크롤! 

전체 일기 보여주는 페이지를 커서 기반 무한 스크롤로 구현하였는데, 이때 window 객체에 scroll 이벤트를 연결하여 특정 지점에 스크롤이 되었을 때, 서버에 다음 일기 리스트를 요청하고 이때 파라미터에 cursor 값을 보내는 형식으로 하였다!    

```js
// handleScroll.js
export const handleScroll = () => {
  const scrollHeight = document.documentElement.scrollHeight;
  const scrollTop = document.documentElement.scrollTop;
  const clientHeight = document.documentElement.clientHeight;
  if (scrollTop + clientHeight >= scrollHeight) {
    return true; // true 값 반환
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
        if (res === true) { // true 값이 반환될 때 
          setIsLoaded(true); // setIsLoaded(true)로 상태 변경
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
        snackBar('info', '더 이상 작성한 일기가 없습니다. ');
      }
    }
  };
```
### 🔥 문제 
1. 요청할 일기 리스트는 더 이상 없지만, 스크롤로 인해 불필요한 요청 발생.
2. 스크롤 이벤트가 발생하면 이벤트가 호출되어 메인 스레드 성능에 좋지 않음.

![image](https://user-images.githubusercontent.com/89979344/180594160-9010a01e-fb4a-4678-846b-d1d4cae8cda0.png)


### 💡 문제 1 해결
#### stop 이라는 상태값 설정 
사용자가 받아온 일기 리스트가 10개 미만이라면(일기 리스트 10개씩 받아옴.)   
더 이상 일기 리스트가 없다는 뜻이기 때문에 stop 상태값을 `true`로 변경!   
스크롤 이벤트가 발생하여도, stop 값이 true 이기때문에 getList 함수 호출 X.
```js
 const [stop, setStop] = useState(false); 
  
  useEffect(() => {
    if (isLoaded && !stop) {  // stop 값이 false 일때만 호출.
      getList();
    }
  }, [isLoaded]);

  useEffect(() => {
    window.addEventListener(
      'scroll',
      function (event) {
        const res = handleScroll(event);
        if (res === true) { // true 값이 반환될 때 
          setIsLoaded(true); // setIsLoaded(true)로 상태 변경
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
     // 만약 서버에서 전달받은 일기리스트가 10개 미만이라면
     // 일기 리스트가 더 이상없다는 뜻 -> stop 상태값 true 로 변경. 
      } catch (err) {
        snackBar('info', '더 이상 작성한 일기가 없습니다. ');
      }
    }
  };
```

문제 1이 완벽하게 해결하진 못했다. 
예를 들어 사용자의 일기가 30개라면? 10 -> 10 -> 10  
길이는 10개 이기때문에 stop 상태값이 변경되지 않는다. 

### 🤔 문제 2 해결 방법은 ?     
      
고민 끝에 `Throttle` 을 사용하여 최적화 작업을 하려고 했다.      

#### 📍 Throttle ?   

이벤트에 의한 콜백을 일정 시간 뒤에 호출.     
이런 성질을 이용해서, scroll 이벤트에 적용했을 때 설정한 시간동안은 스크롤을 하더라도 
콜백이 호출되지 않는다.        

### ⭐️ Intersection Observer API ⭐️  
교차 관찰자 API(Intersection Observer API)는 상위요소 또는 최상위 문서의 뷰포트  와      
대상 요소의 교차점에서 변화를 비동기적으로 관찰할 수 있는 방법을 제공한다.  

쉽게 말해, scroll 이벤트와 달리 타겟 요소가 다른 요소에 들어가거나 나갈 때 or 지정한 만큼 두 요소의 교차 부분이 변경될때마다 등록한 콜백함수를 실행할 수 있다!   
=> 문제 2 해결!    

### ⚙️ 수정한 코드
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
        snackBar('info', '더 이상 작성한 일기가 없습니다. ');
      }
    }
  };
```

---
### 🖇 reference 

https://developer.mozilla.org/ko/docs/Web/API/Intersection_Observer_API
