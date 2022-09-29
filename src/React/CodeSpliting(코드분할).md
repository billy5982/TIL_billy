## <span style = "font-size : 18px">코드 분할(Code Spliting) : 현재 필요한 코드만 불러오고, 필요하지 않은 코드는 나중에

- 대부분의 리액트 앱들은 Webpack,Rollup 같은 툴을 이용해서 번들링 진행
  - 이는 HTML에 쉽게 JavaScript를 추가할 수 있기 때문
- 모던 웹으로 발전되면서 DOM을 다루는 정도가 정교해지고, 코드가 무거워짐 -> 특정 지점에서 코드 해석하고 실행하는 정도가 느려짐

## <span style = "font-size : 18px">번들 분할 혹은 줄이는 법

- 번들 시 서드 파티 라이브러리도 포함됨. 해당 서드파티 라이브러리를 전부 불러오게 되면 일부만 사용하고 사용하지 않게 되는 데 사용하지 않는 라이브러리의 코드들도 같이 번들링이 됨. -> 코드양이 많아짐.(앱 성능 저하) -> 필요한 코드만 import 해서 사용하는 것이 좋음
  예)

```jsx
import _ from "lodash"; // X

// 필요한 라이브러리 메소드만 불러와서 사용하는 것이 앱 성능에 좋음
import find from "lodash/find";
```

## <span style = "font-size : 18px">React에서의 코드 분할(Dynamic Import, React.lazy, React.Suspense)

- 리액트에서의 코드분할이 왜 필요한가?
  - 리액트는 SPA . => 사용하지 않는 컴포넌트들도 한 번에 불러오기 떄문에 첫화면 랜더링이 오래 걸림.
  - 이를 React.lazy와 Suspense를 통해 해당 컴포넌트가 필요할 때만 코드를 가져올 수 있게 변경할 수 있음(Dynamic Import).
  - 앱에 코드분할을 사실상 까다롭기 떄문에, Router로 분기하는 지점에서 사용하면 좋음
- React.lazy : 사용할 컴포넌트를 import 해오는 기능
- React.suspense : lazy를 통해 컴포넌트를 가져올 때 일정 대기시간이 발생됨. fallback props를 이용해서 해당 대기시간에 보여줄 컴포넌트를 지정할 수 있음

```jsx
import { Suspense, lazy } from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
/*
분기되는 지점에서 lazy로 컴포넌트를 불러오고 해당 컴포넌트를 변수에 저장
라우터 element로 해당 컴포넌트를 작성해주면 해당 라우터로 이동될 때 해당 코드를 불러와서 렌더링함
*/
const Home = lazy(() => import("./routes/Home"));
const About = lazy(() => import("./routes/About"));

const App = () => (
  <Router>
    {/* 
    fallback lazy를 통해 컴포넌트를 불러올 때 대기시간이 발생 그에 따른 
    대기시간에 보여줄 컨텐츠를 지정해줄 수 있음
    */}
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```
