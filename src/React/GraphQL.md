## <span style='font-size = 18px'>GraphQL

특징

- 모든 데이터가 그래프로 이어져있음 -> 이는 정렬 방법에 따라 그래프를 트리구조로 변경하여 탐색할 수 있음을 의미
- 클라이언트 요청에 따라 트리구조의 JSON 데이터를 유연하게 응답으로 전송(REST API의 경우는 정해진 URI에 따라 데이터를 전송함)

## <span style='font-size = 18px'>REST API 단점

- overfetch : 특정 데이터가 필요한 상황에서 요청 데이터보다 많은 데이터(불필요한 데이터)가 포함될 수 있음
- underfetch : 필요한 정보가 다르다면 여러번의 요청이 필요함

## GraphQL <-> REST

| GraphQL                                                 | REST                                                               |
| ------------------------------------------------------- | ------------------------------------------------------------------ |
| 형태와 데이터 요청이 분리되어 있음                      | 리소스에 대한 형태 정의와 데이터 요청방법이 연결되있음(정해진 URI) |
| 클라이언트가 필요한 요청만을 가져올 수 있음.            | 리소스 응답 자료 크기형태를 서버에서 정함(REST)                    |
| schema-리소스 / Query,mutation-작업유형                 | URI-리소스 / Method-작업유형                                       |
| 단 한번의 요청으로 여러 리소스 접근 가능                | 여러 리소스 접근 시 여러번의 요청이 필요                           |
| 요청받은 각 필드에 대한 resolver를 호출하여 작업을 처리 |

## GraphQL Keyword

- query : get 메소드
- Mutation : post,delete,update,create를 의미
- subsciption : 구독의 개념, 실시간 업데이트 특정 이벤트 발생시 서버가 대응하는 데이터를 실시간으로 클라이언트에게 전송

```jsx
//오퍼레이션 타입, 오퍼레이션 명(전달인자)
//오퍼레이션 타입에는 query,mutation,subscription
// 전달인자 : 쿼리의 필드 및 중첩된 객체들에 전달하여 원하는 데이터만 가져올 수 있음
// 전달인자 내부 변수 : 동적 인수를 받아 쿼리하는 경우가 대다수 변수는 동적으로 받고 싶을 때 사용
// $변수이름 : 타입 형태로 정의. !가 뒤에 붙는다면 episode는 반드시 Episode여야 함.
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}

// 응답
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}

//스키마 : 서비스에서 가져울 수 있는 객체의 종류, 포함하는 필드를 나타내는 객체 유형
// Character => GraphQL 객체 타입을 의미, 필드가 있는 타입임을 의미
type Character {
    // name 과 appearIn 은 Character 타입의 필드
  name: String!
  //[] 배열을 이미, !를 붙이면 0개 이상의 요소를 포함한 배열을 기대할 수 있음
  appearsIn: [Episode!]!
}

//리솔버(resolver) 요청에 대한 응답을 결정해주는 함수, 실제 일하는 방식 즉 로직을 작성
const db = require("./../db")
const resolvers = {
  Query: { // **Query :** 저장된 데이터 가져오기 (REST 에 GET 과 비슷합니다.)
		getUser: async (_, { email, pw }) => {
			db.findOne({
				where: { email, pw }
			}) ... // 실제 디비에서 데이터를 가져오는 로직을 작성합니다.
			...
		}
  },
  Mutation: { // **Mutation :** 저장된 데이터 수정하기 ( Create , Update , Delete )
		createUser: async (_, { email, pw, name }) => {
			...
		}
  }
  Subscription: { // **Subscription :** 실시간 업데이트
    newUser: async () => {
      ...
		}
  }
};



import { graphql } from "@octokit/graphql";
import { useEffect } from "react";


function App() {

    const [repo,setRepo] = useState({})

    async function a() {
      const { repository } = await graphql(
        `
          query {
            repository(owner: "codestates-seb", name: "agora-states-fe") {
              discussions(first: 1) {
                edges {
                  node {
                    title
                  }
                }
              }
            }
          }
        `,
        {
          headers: {
            authorization: `token 깃허브토큰`,
          },
        }
      );
      return repository;
    }

  useEffect(() => {
    a()
    .then((res)=>setRepo(res))
    .catch((err)=>console.log(err));
  }, []);

  return <div>asdf</div>;
}

export default App;
```
