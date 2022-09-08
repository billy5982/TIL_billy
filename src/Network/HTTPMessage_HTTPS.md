## HTTPS & HTTP

## HTTP : Hyper Text Transfer Protocol
- 무상태성 : 서버가 클라이언트에 상태를 기억하고 있지 않음.
    - 예) 상태를 기억한다 했을때  
    손님(클라이언트) -> 점원(서버)  
    가격 문의를 하면? -> 대답해줌  
    2개 살게요 하면 -> 결제 방식을 물어봄(해당 가격문의에 대한 건을 답해줌)
    신용카드요 하면 -> 결제를 해줌  
    - 예) 상태를 기억하지 않으면?  
    손님(클라이언트) => 점원(서버)  
    가격 문의 ->  얼마입니다.  
    2개 살게요 ->  ?? 뭐를요?  
    신용카드로 결제요 ->  ?? 뭐냐니깐요??
    - 상태를 기억하면 손님이 편하게 할 수 있음 대신, 중간에 점원이 바뀐다면?? 새로운 점원은 뭔지 모름,,,
    - 상태를 기억하지 않고 손님이 정보를 한꺼번에 말한다면??   
    어떤 점원이 와도 해당 건수를 처리할 수 있음
    - **장점**  
    **클라이언트는 요청을 한번에 보내주고, 처리하는 서버가 엄청나게 많다면??**  
    해당 내에 있는 모든 서버 아무거나가 해당 요청을 처리할 수 있음-> **서버 무한 증설 가능**  
    **하지만 상태를 기억한다면???**  
    해당 서버 컴퓨터가 아니면 해당 요청을 처리해줄 수 없음
- 비연결성
    - 요청과 응답을 받을 때만 연결을 유지하고 해당 상황이 끝나면 연결을 끊어줌
    ![스크린샷 2022-09-08 오후 4 29 50](https://user-images.githubusercontent.com/104412610/189081854-1e4fd9bd-7c6f-4874-b568-9f02fc8a7f34.png)

## HTTP 헤더의 종류와 특징
HTTP 메세지는 헤더와 바디로 구분
<img src = 'https://hanamon.kr/wp-content/uploads/2021/06/HTTP_messages.png'>  
 **Accept** : 콘텐츠 협상 헤더측에 속함  
 **Content** : 표현 헤더에 속함.

- HTTP 바디
    - 메세지 본문을 통해 표현 데이터를 전달  
    - 메세지 본문(데이터를 실어 나름) = payload
    - 표현은 요청이나 응답에서 전달할 실제 데이터
    - **표현 헤더는 표현 데이터를 해석**할 수 있는 **정보**를 제공
- HTTP 표현 헤더 : 표현 데이터의 형식, 압축 방식, 자연 언어, 길이 등을 설명하는 데이터, 표현 헤더는 응답 요청 둘 다 사용
- **공통 헤더**
    - Content-Type : 표현 데이터의 형식   
    text/html;charset=UTF-8   
    application/json  
    image/png
    - Content-Encoding : 표현 데이터의 압축 방식  
    표현 데이터를 압축하기 위해 사용,  
    데이터를 전달하는 곳에서 압축 후 인코딩 인코딩 해더 추가,  
    데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
    - Content-Language : 표현 데이터의 자연 언어(표현 데이터의 자연 언어를 표현)
    - Content-length : 표현 데이터의 길이(바이트 단위)
- **요청 헤더**
    - From : 유저 이메일 정보
        - 일반적으론 잘 사용하지 않음
        - 검색엔진에서 주로 사용
    - Referer : 이전 페이지의 웹페이지 주소
        - 현재 요청된 페이지의 이전 웹 페이지 주소
        - A -> B Referer : A를 포함해서 요청
        - 이용하면 유입경로 수집 가능
    - User-Agent : 유저 애플리케이션 정보
        - 어떤 종류 브라우저에서 장애가 발생하는 지 파악 가능
    - Host : 요청한 호스트 정보(도메인)
        - 필수 헤더
        - 하나의 서버가 여러 도메인을 처리해야 할 때 호스트 정보를 명시하기 위해 사용
        - 하나의 IP주소에 여러 도메인 적용되있을 때 호스트 정보를 명시하기 위해 사용
    - Origin : 서버로 POST 요청 시 요청을 시작한 주소를 나타냄
        - 기재하지 않으면 CORS 에러
        - 응답에 Access-Control-Allow-Origin과 관련
    - Authorization : 인증 토큰을 서버로 보낼 때 사용
- **응답 헤더**
    - Server : 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
    - Date : 메세지가 발생한 날짜, 시간
    - Location : 페이지 리디렉션
        - 3xx응답 결과에 Location헤더가 있으면 웹 브라우저는 해당 위치로 리다이렉트
        - 201 : Location 값은 요청에 의해 생성된 리소스 URI
    - Allow : 허용 가능한 HTTP 메서드
    - Retry-After : 유저가 다음 요청을 하기전까지 기다려야 하는 시간

##### 콘텐츠 협상 헤더
언어를 예로 들었을 때, 서버에서 해당 언어를 지원하지 않을 수 있음. 기본 언어가 독일어라면?? 독일어를 응답받을 수 있음.  
그래서 우선순위를 정하는 것
- 1부터 0까지 우선순위를 부여하면 이를 토대로 서버는 응답을 지원함. 1과 가까울수록 우선순위가 높음
- Accept-Language : ko-KR;q=1,ko;q =0.9,en-US;q=0.8 이런 식

 
