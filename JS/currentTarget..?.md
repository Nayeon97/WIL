## π currentTarget vs target

μμ±μΌ: 2022λ 5μ 12μΌ

### π νμλλ‘ e.target.valueλ₯Ό μ΄μ©νλλ°, ν΄λ¦­ν μμκ°μ κ°μ Έμ€μ§ λͺ»νλ€..?π¬<br />

2μ°¨ ννλ‘μ νΈμμ λ°μν λ¬Έμ μ μ΄λ€...!! νμμ²λΌ e.target.value λ₯Ό μ΄μ©ν΄μ ν΄λ¦­ν μμλ₯Ό κ°μ Έμ¬λ €κ³  νμΌλ, undefined μ΄ μΆλ ₯λμλ€.  
λ€νν κ΅¬κΈλ§μ ν΅ν΄μ μμλΈ e.currentTarget.value λ₯Ό μ¬μ©νλ λ¬Έμ λ ν΄κ²°λμλ€.  
κ·ΈλΌ event target μ currentTarget μ°¨μ΄μ μ λ¬΄μμΌκΉ..?

<img src="../imgs/currentTarget1.png" width="600" height="60"/>

### β event.target vs event.currentTarget

λΈλ‘κ·Έμ κΈ°μ¬λμλ μ½λλ₯Ό λ¨Όμ  μ€νν΄λ³΄μ. (μΆμ² μλμ κΈ°μ¬)

```html
 <body>
    <tbody>
      <div class="green" style="width: 300px; height: 300px; background-color: green;">
          <div class="yellow" style="width: 200px; height: 200px; background-color: yellow;"></div>
      </div>
      </tr>
    </tbody>
    <script>
        const green = document.querySelector('.green');
        green.addEventListener('click',function(e){
            console.log("currentTarget : ",e.currentTarget);
            console.log("Target : ",e.target);
        })
    </script>
  </body>
```

μμ μ½λλ₯Ό μ€ννκ³ , yellowμ ν΄λ¦­νμλ κ²°κ³Όλ μλμ κ°μ΄ λμ¨λ€.  
green μ΄λΌλ className μ κ°μ§ divμ μ΄λ²€νΈ νΈλ€λ¬λ₯Ό μ€μ νμλλ°, currentTarget μ μκΈ°μμ (μ΄λ²€νΈκ° λ¬λ €μλ μμ)κ³Ό μμμμκΉμ§ μΆλ ₯λλ κ²μ, target λ μμμμ(λ΄κ° λλ₯Έ λμ!-> yellow)λ§ μΆλ ₯λλ κ²μ λ³Ό μ μλ€.
<img src="../imgs/currentTarget2.png" width="800" height="130"/>

κ·ΈλΌ μ€λ₯κ° λ°μνλ νλ‘μ νΈ μ½λλ₯Ό μμλ‘ λ€μ΄λ³΄μ. μΌλ¨ μ½λκ° λ³΅μ‘ν΄μ μμλ‘ κ°λ¨νκ² λ§λ€μλ€.

```js
const SurveyContainer = () => {
  const handleClickAnswer = (e) => {
    console.log('e.target.value', e.target.value);
    console.log('e.currentTarget.value', e.currentTarget.value);
  };

  return (
    <>
      <div>
        <button onClick={handleClickAnswer} value={'buttonμ value'}>
          <span>click</span>
        </button>
      </div>
    </>
  );
};
```

e.target.value λ₯Ό νμ λ undefined κ° μΆλ ₯λλ κ²μ λ³Ό μ μλ€! μλνλ©΄ μ΄λ²€νΈ νΈλ€λ¬κ° λ¬λ €μλ button μ value κ°μ μ§μ νλλ°, e.target.value λ₯Ό μ¬μ©νλ©΄, λ΄κ° ν΄λ¦­ν λμμΈ spanμ value κ°μ μΆλ ₯νλ €κ³  νκΈ°λλ¬Έμ΄λ€..(λ¬Όλ‘  span μλ value κ°μ μ°μ§ μλλ€..!)

<img src="../imgs/currentTarget3.png" width="300" height="60"/>

β οΈ **λ€μ νλ² μ λ¦¬νλ©΄**
β event.target : λ΄κ° λλ₯Έ λμ! , μ΄λ²€νΈκ° λ¬λ €μλ μμμ μμ μμ.
β evnet.currentTarget : μ΄λ²€νΈκ° λ¬λ € μλ μμ!

μΆκ°μ μΌλ‘ MDN μ μλ Event.currentTarget μ λν μ€λͺμ μ½μ΄λ³΄λ©΄, λμΌν μ΄λ²€νΈνΈλ€λ¬λ₯Ό μ¬λ¬ μμμ μ¬μ©ν  λ ν¨κ³Όμ μ΄λΌκ³  νλ€!

---

https://developer.mozilla.org/en-US/docs/Web/API/Event/currentTarget  
https://kyounghwan01.github.io/blog/JS/JSbasic/target-currentTarget/
