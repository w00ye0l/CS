# HTTP (HyperText Tranfer Protocol)

## 0) 정의

> 클라이언트-서버 프로토콜 (보통 웹 브라우저인 수신자 측에 의해 요청이 초기화되는 프로토콜) : 클라이언트와 서버 간에 요청/응답으로 데이터를 주고 받을 수 있는 프로토콜
>
> HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜
>

<br/>

### 0-0) 프로토콜이란?

> 통신 프로토콜 또는 통신 규약은 컴퓨터나 원거리 통신 장비 사이에서 메시지를 주고 받는 양식과 규칙의 체계
>
> 통신 프로토콜은 신호 체계, 인증, 그리고 오류 감지 및 수정 기능을 포함할 수 있음

<br/>

## 1) 특징

- HTTP는 웹에서 이루어지는 모든 데이터 교환의 기초

- 요청은 하나의 개체, 사용자 에이전트(또는 그것을 대신하는 프록시)에 의해 전송
  - 대부분의 경우, 사용자 에이전트는 브라우저지만, 어떤 것이든 될 수 있음 (예로, 검색 엔진 인덱스를 채워넣고 유지하기 위해 웹을 돌아다니는 로봇)

<br/>

## 2) HTTP 메시지

### 2-1) 요청

- **HTTP 메서드**, 보통 클라이언트가 수행하고자 하는 동작을 정의한 `GET`, `POST` 같은 동사나 `OPTIONS`나 `HEAD`와 같은 명사, 일반적으로, 클라이언트는 리소스를 가져오거나(`GET`을 사용하여) HTML 폼의 데이터를 전송(`POST`를 사용하여)하려고 하지만, 다른 경우에는 다른 동작이 요구될 수 있음
- **가져오려는 리소스의 경로**; 예를 들면 프로토콜 (`http://`), 도메인 (`developer.mozilla.org`), 또는 TCP 포트 (`80`)인 요소들을 제거한 `리소스의 URL`
- **HTTP 프로토콜의 버전**
- **서버에 대한 추가 정보를 전달하는 선택적 헤더들**
- `POST`와 같은 몇 가지 메서드를 위한, **전송된 리소스를 포함하는 응답의 본문과 유사한 본문**

<br/>

### 2-2) 응답

- **HTTP 프로토콜의 버전**
- **요청의 성공 여부와, 그 이유를 나타내는 상태 코드**
- 아무런 영향력이 없는, **상태 코드의 짧은 설명을 나타내는 상태 메시지**
- 요청 헤더와 비슷한, **HTTP 헤더들**
- 선택 사항으로, **가져온 리소스가 포함되는 본문**

<br/>

## 3) HTTP 메서드

### 3-1) GET

> 서버로부터 정보를 조회하기 위해 설계된 메서드

- 요청을 전송할 때 필요한 데이터를 `Body`에 담지 않고, **쿼리스트링**을 통해 전송한다.
  - `쿼리스트링` : URL 끝에 `?`와 함께 `key: value`의 쌍으로 이루어진 요청 파라미터
  - 요청 파라미터가 여러 개이면 `&`로 연결
  - 예시) `www.example.com/resource?name1=value1&name2=value2`

- **쿼리스트링**을 사용하면 URL에 조회 조건을 표시하기 때문에 특정 페이지를 링크하거나 북마크할 수 있음
- 불필요한 요청을 제한하기 위해 요청이 캐시될 수 있음
  - JS, CSS, 이미지 같은 정적 컨텐츠는 데이터의 양이 크고 변경될 일이 적어 동일한 요청을 반복해서 보낼 필요가 없어 **브라우저에서는 요청을 캐시해두고 캐시된 데이터를 사용**
  - 개발 중에 정적 컨텐츠를 변경해도 내용이 바뀌지 않는 경우 `캐시를 지운 후 다시 조회`. 캐시를 지우면 다시 컨텐츠를 조회하기 위해 서버에 요청을 보냄

<br/>

### 3-2) POST

> 리소스를 생성/변경하기 위해 설계된 메서드

- 데이터를 `HTTP 메시지의 Body`에 담아 전송
  - `HTTP 메시지의 Body`는 길이의 제한 없이 데이터 전송 가능 => `GET`과 달리 **대용량 데이터 전송 가능**
- `POST`의 경우 `GET`과 달리 눈에 보이지 않아 보안적인 면에서 안전해보이지만, 크롬 개발자 도구, Fiddler와 같은 툴로 요청 내용을 확인할 수 있어 반드시 `암호화`하여 전송해야 함
- `POST`로 요청을 보낼 때는 요청 헤더의 `Content-Type`에 요청 데이터의 타입을 표시해야 함
  - 표시하지 않으면 서버는 내용이나 URL에 포함된 리소스의 확장자명 등으로 데이터 타입 유추
  - 알 수 없는 경우 `application/octet-stream`으로 요청 처리

<br/>

### 3-3) GET vs. POST

| 메서드 | 설계 방식        | 의미                                                         | 사용                                    | 사용 예시                                                    |
| ------ | ---------------- | ------------------------------------------------------------ | --------------------------------------- | ------------------------------------------------------------ |
| `GET`  | Idempotent(멱등) | 서버에게 동일한 요청을 여러 번 전송하더라도 동일한 응답이 돌아와야 한다는 것 | 주로 조회를 할 때 사용                  | 브라우저에서 웹페이지를 열어보거나 게시글을 읽는 등 조회를 하는 행위에 사용 |
| `POST` | Non-Idempotent   | 서버에게 동일한 요청을 여러 번 전송해도 응답은 항상 다름     | 서버의 상태나 데이터를 변경시킬 때 사용 | 게시글을 쓰면 서버에 게시글이 저장되고, 게시글을 삭제하면 데이터가 없어지는 변경되는 행위에 사용 |

#### 3-3-0) Idempotent (멱등)

> 수학이나 전산학에서 연산의 한 성질을 나타내는 것
>
> 연산을 여러 번 적용하더라도 결과가 달라지지 않는 성질

<br/>

#### 3-3-1) 보다 나은 POST 사용

> `POST`는 생성, 수정, 삭제에 사용할 수 있지만, 생성에는 `POST`, 수정은 `PUT` 또는 `PATCH`, 삭제는 `DELETE`가 용도에 더 맞는 메서드