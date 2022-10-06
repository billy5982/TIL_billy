## TDD(Test-driven Development)

- 코드 작성 전 테스트코드를 먼저 작성하는 소프트웨어 개발 방법론.
- 자신이 바람직하다고 생각하는 코드의 결과를 미리 정의하고, 결과를 바탕으로 코드를 작성하는 방법
- 테스트 코드를 먼저 작성한다는 것은 필연적으로 코드를 어떻게 구성할 지 고민하게 된다는 것을 의미.
  - 예상하지 못했던 버그를 줄여 소요 시간을 줄일 수 있음.
  - TDD에 따라 소프트웨어 개발 시 그렇지 않은 경우보다 결함을 50~90% 감소시킬 수 있음.

1. 실패하는 테스트 코드(테스트 케이스)를 먼저 작성
   - 1의 과정을 진행 중 2의 작업을 시작하지 않도록 주의
2. 테스트 코드를 성공시키기 위한 실제 코드 작성
   - 1의 테스트를 통과할 정도로의 최소 코드만 작성
3. 중복 코드를 제거, 일반화 등의 리팩토링 수행

테스트 코드 : `describe`,`it`,`assert`,`expect` 을 이용

- 해당 테스트 관련 메소드는 테스트 오픈소스 프레임워크를 이용한 것. 예) mocha(테스트 프레임워크),chai(라이브러리)

## JS TDD

```js
//내가 작성한 함수 work.js
function work(card){
    if(card~~~){
        return '결과'
    }
}

//work.test.js(mocha 테스트) 특성 : 테스트케이스 실행시 오류가 발생하면 실패, 오류가 발생하지 않으면 성공으로 간주
describe("테스트에 대한 설명 describe안에는 여러가지 테스트를 넣을 수 있음",()=>{
    //it(테스트 설명, 테스트 함수)
    it("오류가 발생 시 테스트가 실패",()=>{
        let even = (num)=> num % 2===0; // 짝수 판별 함수
        if(even(10)!== true){
            // 에러가 발생하면 해당 테스트는 실패로 간주됨. 실패할 테스트 케이스를 Error로 처리해줄 것.
            throw new Error("10은 짝수여야 합니다");
        }
    })

    it('38로 시작되고 길이가 14여여만 합니다', ()=>{
        // 해당 함수를 실행했을 때 틀렸을 경우 해당 if문이 실행되고 if문에서 error를 보내줬기 떄문에
        // 해당 케이스는 인자에 따라서 실패할 수도 있음 if문 false가 된다면 해당 테스트 케이스는 통과처리
        if(word('테스트할 예시를 넣어줍니다') !=='Diner Party'){
            throw new Error("test fail");
        }
    })

    // 항상 throw와 if로 테스트 케이스를 실패 처리하는 것은 귀찮은 일이기 때문에 해당 함수 제작
    let assert = function (isTrue) {
    if (!isTrue) {
      throw new Error("Test failed");
    }
  };
    it("38로 시작되고 길이가 14여여만 합니다",()=>{
        // 함수 인자로 전달된 값이 true or false에 따라 error가 발생하여 테스트가 실패할지 성공할지를 결정함
        assert(word('테스트할 예시를 넣어줍니다') !=='Diner Party')
    })

    // 위에 assert함수는 chai 라이브러리에서 가져와 사용할 수 있음.
    // chai 라이브러리는 테스트에 필요한 헬퍼 함수들이 담긴 라이브러리
    let assert = chai.assert;
    it("has a prefix of 4 and a length of 13", function () {
    assert(detectNetwork("4123456789012") === "Visa");
    });
    // should
    let assert = chai.should;
    it("has a prefix of 4 and a length of 13", function () {
    should.equl(detectNetwork("4123456789012"),"Visa");
    });
    //expact
    let expect = chai.expect;
    it(FILL_ME_IN, function () {
    expect(detectNetwork("5112345678901234")).to.equal("MasterCard");
    });
})

```

## React TDD

- CRA(create-react-app)을 통해 생성된 프로젝트는 Test Library를 사용 가능
- Jest : JS의 testing framwork,Test Runner -> 테스트 파일 자동 감지하여 테스트를 함
  - 함수를 이용하여 테스트가 기대값 만큼 올바른 값을 가지고 있는 지 확인한다
- package.json(리액트)
  - testing-library/jest-dom : jest dom 제공 custom matchers 사용 가능
  - testing-library/react : component 요소 찾기 위한 query 포함
  - testing-library/uesrEvent : Click 등 사용자 이벤트를 포함

```jsx
// 컴포넌트 제작 Light.js
import React, { useState } from "react";

function Light({ name }) {
  const [light, setLight] = useState(false);

  return (
    <div>
      <h1>
        {name} {light ? "ON" : "OFF"}
      </h1>
      <button onClick={() => setLight(!light)} disabled={light ? true : false}>
        On
      </button>
      <button onClick={() => setLight(!light)} disabled={!light ? true : false}>
        OFF
      </button>
    </div>
  );
}

export default Light;

// 같은 폴더에 테스트 파일 제작 Lights.test.js
import { render, screen, fireEvent } from "@testing-library/react";
import Light from "./Light";

//Light 컴포넌트에 props로 전원을 내려줬을 때 해당 컴포넌트가 props를 화면에 잘출력하고 있는 지 확인하는 것
it("renders Lights Components", () => {
  //테스트하고자 하는 컴포넌트를 render를 통해 전달
  render(<Light name="전원"></Light>);
  //screen.getByText() => 해당 컴포넌트(App)가 전원 off라는 내용을 담고 있는 태그가 있는 지 확인하고 nameElement에 할당
  const nameElement = screen.getByText(/전원 off/i);
  // toBeInTheDocument() => document.body에 해당 nameElement(전원 off를 가진 태그가 있는 지 확인한다)
  expect(nameElement).toBeInTheDocument();
});

it("off btn disabled", () => {
  render(<Light name="전원" />);
  //getByRole로 button을 지정, 옵션을 이용하여 off 내용을 가지고 있는 버튼을 찾는다. <button>OFF</button>
  const offBtnElement = screen.getByRole("button", { name: "OFF" });
  // 해당 버튼이 disabled인지 확인한다.
  //matcher함수 expact 함수에 지정값이 toBe 함수 내에 값과 일치하는 지 체크한다
  expect(offBtnElement).toBeDisabled();
});

it("off btn enable", () => {
  render(<Light name="전원"></Light>);
  const onBtnEnable = screen.getByRole("button", { name: "On" });
  //   expect(onBtnEnable).toBeEnabled();
  //not을 붙여서 반대 테스트를 진행할 수도 있음
  expect(onBtnEnable).not.toBeDisabled();
});

// 버튼 클릭 이벤트의 유무를 테스트로 구현할 수 있음(fireEvent)
it("change from off to on", () => {
  render(<Light name={"전원"} />);
  const onBtnElement = screen.getByRole("button", { name: "On" });
  fireEvent.click(onBtnElement);
  expect(onBtnElement).toBeDisabled();
});
```
