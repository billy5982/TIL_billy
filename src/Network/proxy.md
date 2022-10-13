## CORS

- 동일 출처정책으로 인해 API와 현재 주소가 일치해야한 데이터에 접근할 수 있었음.
- 다른 도메인에서 API 요청을 하려면 CORS 설정이 필요함.
- 출처 : 접근할 때 사용하는 사용자의 프로토콜,호스트(도메인),포트로 정의됨.

## 라이브 데이터

- 실제 서비스되고 있는 앱의 데이터베이스에 적재되고 있는 데이터, 유저 및 상품,결제 등 다양한 정보들을 예로 들 수 있음.
- 민감성이 높은 데이터들이 위주이기 때문에 보안이 무엇보다 중요함.

## Proxy

- 위처럼 CORS를 설정하는 정석적인 과정없이 React 라이브러리, Webpack Dav Server에서 제공하는 proxy 기능을 사용하면 CORS 정책을 우회할 수 있음
- 별도의 응답헤더 없이 브라우저는 React 앱으로 데이터 요청 -> 백엔드로 전달, React 앱이 서버로부터 받은 응답 데이터 -> 브라우저로 전달하는 방법을 사용하기 때문에 브라우저는 CORS 정책을 위반한 지 모름

## 내가 생각하는 proxy

- 브라우저에서 요청을 보내면 리액트 앱이 백엔드로 요청을 전달하구, 백엔드는 해당 요청을 리액트 앱으로 보낸다. 그리고 서버의 응답을 받은 리액트앱은 브라우저에 표시된다?(proxy)
- 원래는 요청을 보내면 브라우저가 백엔드로 요청을 보내주고 데이터를 받을때 브라우저에서 CORS 설정을 확인해서 백엔드 데이터를 받고 웹에 표현해주는 방식?

## webpack dev server proxy

- 브라우저 API를 요청할 때 백엔드 서버에 직접적으로 요청하지 않고, 현재 개발서버의 주소로 우회 요청을 하게 됨
- 웹팩 개발서버의 proxy 설정은 원래 웹팩 설정을 통해 적용, **CRA를 통해 만든 리액트 프로젝트는 package.json에서 proxy값을 설정**

```json
// 보통 package.json 제일 아래에 배치
"browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
	"proxy" : "우회할 API 주소"
}
```

```jsx
//fetch 혹은 axios를 통해 요청하던 부분에 도메인을 제거
export async function getfetch() {
    const response = await fetch('/params');
    .then(() => {
			...
		})
}
```

```jsx
// React proxy 설정 방법 => webpack dev server에서 설정한 proxy는 전역적인 설정이므로 해당 방법이 적용되지 않는 경우가 종종 발생
npm install  http-proxy-middleware --save
// 해당라이브러를 사용할 때는 요청에 /api/요청할 곳 작업해야한다. use가 두개 이상일 경우가 존재하기 때문
// ./src/setupProxy.js
const {createProxyMiddleware} = require('http-proxy-middleware');
module.exports =function(app){
    app.use(
        '/api', // 프록시가 필요한 path parameter 입력
    createProxyMiddleware({
      target: 'http://localhost:5000', //타겟이 되는 api url를 입력합니다.
      changeOrigin: true, //대상 서버 구성에 따라 호스트 헤더가 변경되도록 설정하는 부분
    })
    )
}
```
