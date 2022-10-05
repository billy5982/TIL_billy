## React Custom Hook

- 코드 작성 시 중복되는 코드가 발생될 때가 많다  
  예) input의 상태관리 + 상태 업데이트 함수, 네트워크 호출-> 데이터를 전달받는 등
- 이때 여러 번 중복되는 함수를 Custom hook으로 만든다면?? 코드의 중복을 피할 수 있다.
- src 안에 hooks 디렉토리를 만들고 원하는 hook을 제작하면 되고, react hook도 사용할 수 있다. hook 이름은 앞에 use를 붙여줘야 한다.

**useInput 특징 : 상태와 상태를 관리하는 이벤트를 같이 return으로 전달해준다.**

```jsx
// 이런 식으로 여러 인풋이 모여있는 창이라 가정했을 때 해당 인풋에 대한 상태와 해당 상태를 변경해주는 이벤트함수가 따로 존재해야한다.
// 이렇게 될 경우 코드의 가독성 및 코드의 중복이 발생할 수 있다.
import { useState } from "react";

function App() {
  const [firstname, setFirstname] = useState("");
  const [secondName, setsecondName] = useState("");
  const [thirdName, setthirdName] = useState("");

  const inputHandler1 = (e) => {
    setFirstname(e.target.value);
  };
  const inputHandler2 = (e) => {
    setFirstname(e.target.value);
  };
  const inputHandler3 = (e) => {
    setFirstname(e.target.value);
  };

  return (
    <>
      <div>
        <input type="text" value={firstname} onChange={inputHandler1} />
        <input type="text" value={secondName} onChange={inputHandler2} />
        <input type="text" value={thirdName} onChange={inputHandler3} />
      </div>
    </>
  );
}
```

**custom hook**

```jsx
// input 상태관리 로직은 회원가입, 로그인 등에서도 자주 사용되는 로직이므로 로직을 분리해주는 것이 좋다.
//그래야 다른 페이지들에서도 사용하기 편하다.
// src/hooks/useInput.js
import { useState } from "react";

export default function useInput(initalValue) {
  //초기값을 지정해주면 좋다
  const [input, setInput] = useState(initalValue);

  const inputHandler = (e) => {
    setInput(e.target.value);
  };

  return [input, inputHandler];
}

// src/App.js
import useInput from "./hooks/useInput";

export default function App(){
    // firName에는 custom hook에 input 상태와 firHandler에는
    // 해당 firName의 상태를 변경하는 이벤트 함수가 들어간다
    const [firName, firHandler] = useInput('');
    const [secName, secHandler] = useInput('');
    const [thiName, thiHandler] = useInput('');

    return (
    <>
      <div>
        <input type="text" value={firName} onChange={firHandler} />
        <input type="text" value={secName} onChange={secHandler} />
        <input type="text" value={thiName} onChange={thiHandler} />
      </div>
    </>
    )
}
```

```jsx
import { useEffect, useState } from "react";

export default function App() {
  const [data, setData] = useState([]);

  //Effect로 서버에 요청을 보낸다 가정
  //네트워크 요청은 어디서나 발생할 수 있다.
  useEffect(() => {
    fetch("data.json", {
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
    })
      .then((response) => {
        return response.json();
      })
      .then((myJson) => {
        setData(myJson);
      })
      .catch((error) => {
        console.log(error);
      });
  }, [data]);

  return (
    <>
      <div className="todo-list">
        {data &&
          data.todo.map((el) => {
            return <li key={el.id}>{el.todo}</li>;
          })}
      </div>
    </>
  );
}
```

**useFetch 특징 : 네트워크 통신 시 어떤 url과 어떤 요청이 들어가는 지 확인한다.**

```jsx
//hooks/useFetch.js
import { useEffect, useState } from "react";
// 해당 custom hook은 url과 header를 인자로 전달받고, 네트워크에 해당 인자로 요청해서
// 해당 통신을 완료한 데이터를 반환하는 구조이다.
export default function useFetch(url, header) {
  const [data, setData] = useState([]);
//의존성 배열은 바뀔 수 있는 url,header로 만들어줘야한다.
  useEffect(() => {
    fetch(url, header)
      .then((response) => {
        return response.json();
      })
      .then((myJson) => {
        setData(myJson);
      })
      .catch((error) => {
        console.log(error);
      });
  }, [url,header]);

  return data;
}

//App.js
import useFetch from './hooks/useFetch'
export default function App(){
    // 통신할 url과 헤더 정보를 보내준다. data에는 해당 useFetch가
    // 리턴하는 값(네트워크 통신 데이터)이 들어오게 된다
    const data = useFetch("data.json", {
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
    })

    return (
        <div>
         {data &&
          data.todo.map((el) => {
            return <li key={el.id}>{el.todo}</li>;
          })}
        </div>
    )
}
```

커스텀 훅 예시

```jsx
// Input.js
function Input({ labelText, value }) {
  return (
    <div className="name-input">
      <label>{labelText}</label>
      <input {...value} type="text" />
    </div>
  );
}

export default Input;

// useInput.js
import { useState } from "react";

function useInput(initialValue) {
  const [value, setValue] = useState(initialValue);

  const reset = () => {
    setValue(initialValue);
  };

  const bind = {
    value,
    onChange: (e) => {
      setValue(e.target.value);
    }
  };

  return [value, bind, reset];
}

export default useInput;

//App.js
import { useState } from "react";
import Input from "./component/Input";
import "./styles.css";
import useInput from "./util/useInput";

export default function App() {
  const [firstValue, firstBind, firstReset] = useInput("");
  const [secondValue, secondBind, secondReset] = useInput("");
  const [nameArr, setNameArr] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    setNameArr([...nameArr, `${firstValue} ${secondValue}`]);
    firstReset();
    secondReset();
  };

  return (
    <div className="App">
      <h1>Name List</h1>
      <div className="name-form">
        <form onSubmit={handleSubmit}>
          <Input labelText={"성"} value={firstBind} />
          <Input labelText={"이름"} value={secondBind} />
          <button>제출</button>
        </form>
      </div>
      <div className="name-list-wrap">
        <div className="name-list">
          {nameArr.map((el, idx) => {
            return <p key={idx}>{el}</p>;
          })}
        </div>
      </div>
    </div>
  );
}

```
