## Component & Hooks

- Class Component
  - 복잡할수록 가시성이 떨어지고, 이해하기가 어려워짐
  - 컴포넌트 사이 상태로직 재활용성이 떨어짐
  - 자바스크립트 this에 동작 방식을 알아야함
- Function Component
  - 값의 사용, 최적화 기능이 떨어짐 -> Hook의 개념 도입하여 보완
  - 클래스 Component의 비해 직관적
- Hook
  - function component에서 사용됨
  - 상태값, 리액트의 여러 기능을 사용하기 편리하게 해주는 메서드임.
- Hook 사용 조건
  - 상태를 리액트 함수 최상위에서만 호출(조건문,반복문,함수에서 호출 X)
  - 리액트 내에서만 실행될 것, Javascript 내에선 실행 X

## useMemo , useCallback , Memoization

- useMemo : 리턴되는 값을 메모리에 저장, defendency array가 변경 될 경우에만 해당 콜백 함수가 실행되고 리턴되는 값이 메모리에 저장됨.
- useCallback : 함수를 메모리에 저장, props를 함수로 내려주는 경우 사용, 함수도 객체이기 때문에 렌더링이 다시 될때마다 다시 정의됨.
- Memoization : 동일한 값을 리턴하는 함수를 반복적으로 호출할 경우, 계산시 해당
  값을 메모리에 저장해서, 필요할 때마다 초기화하는 것이 아닌 메모리에서 꺼내 쓰는 것
- 함수형 컴포넌트 -> 랜더링 -> 내부 변수,함수 다시 실행 -> 재 랜더링되면 동일한 과정을 반복하게 됨. 만약 무거운 함수가 다시 실행된다면? 그만큼 실행시간이 오래걸리게 될 것.

## useMemo(()=>저장될 상태, [])

- 상태가 객체타입일 경우
  - DOM의 랜더링 방식, 원시타입 객체타입의 속성을 알아야 함
    - DOM의 경우에는 이전 DOM과 새로운 DOM을 비교함
    - 원시타입은 값 그자체이므로 같은 값은 같다고 표시함. But 객체는 주소가 할당되고 해당 주소를 비교하기 때문에 새로운 DOM과 이전 DOM을 비교했을 때 객체의 주소가 다르기 때문에 렌더링을 다시하게 됨.
- 상태를 이용한 무거운 함수가 존재할 경우
  - state를 2개 이상 사용할 경우 state가 변경될 때 마다 랜더링이 다시 된다.
  - 하지만 state를 이용한 무거운 함수가 존재할 경우? 다른 state가 변경될 때도 무거운 함수가 실행되어 실행시간이 오래걸리게 된다. 해결책은 useMemo를 사용하여 해당 무거운 함수의 state가 변경될 때만 해당 함수를 실행시켜주면 된다

```jsx
    useMemo(()=>{
        return 기억할 상태 값
    },[defendency Array])

    defendecny Array가 변경될 때만 해당 useMemo의 값이 변경된다.

    import React, { useState,useMemo } from "react";
import "./styles.css";
import { add } from "./add";

export default function App() {
  const [name, setName] = useState("");
  const [val1, setVal1] = useState(0);
  const [val2, setVal2] = useState(0);

  const answer = useMemo(()=>{
    //memo를 사용하지 않으면 name이 변경될 때도 해당 add함수가 실행된다.
    //val1,val2가 변경될 때만 해당 함수가 실행되어서 랜더링을 할수 있게 해줘야
    //한다. usememo가 최종 리턴하는 값은 add함수의 리턴 값임.
    return add(val1, val2)
  },[val1,val2])

  return (
    <div>
      <input
        value={name}
        type="text"
        onChange={(e) => setName(e.target.value)}
      />
      <input
        value={val1}
        type="number"
        onChange={(e) => setVal1(Number(e.target.value))}
      />
      <input
        value={val2}
        type="number"
        onChange={(e) => setVal2(Number(e.target.value))}
      />
      <div>{answer}</div>
    </div>
  );
}

function Calculator({value}){
	const result = useMemo(() => calculate(value), [value]);
    // props로 내려오는 value의 값이 바뀔 때만 랜더링이 다시 됨.
	return  <div>{result}</div>
}

```

## useCallback(()=>{} : 저장할 함수, [])

- props로 함수를 내려줄 때 주로 사용된다. 자바스크립트에선 함수도 객체이기 떄문에 렌더링이 다시 될때마다 함수를 다시 선언한다.

```jsx
export default function App() {
  const [input, setInput] = useState(1);
  const [light, setLight] = useState(true);

  const theme = {
    backgroundColor: light ? "White" : "grey",
    color: light ? "grey" : "white",
  };

  // 버튼을 클릭하면 서로 상관이 없는 데 랜더링이 다시 되면서 해당함수가 재정의 됨
  // 함수는 state가 변경되면 list 컴포넌트에서 해당 값을 받아서 랜더링함
  const getItems = useCallback(() => {
    return [input + 10, input + 100];
  }, [input]);

  const handleChange = (event) => {
    if (Number(event.target.value)) {
      setInput(Number(event.target.value));
    }
  };

  return (
    <>
      <div style={theme} className="wall-paper">
        <input
          type="number"
          className="input"
          value={input}
          onChange={handleChange}
        />
        <button
          className={(light ? "light" : "dark") + " button"}
          onClick={() => setLight((prevLight) => !prevLight)}
        >
          {light ? "dark mode" : "light mode"}
        </button>
        <List getItems={getItems} />
      </div>
    </>
  );
}
```
