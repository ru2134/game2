아이템 시뮬레이터 
### [API 명세서] https://www.notion.so/Node-js-API-bb13ce4f4e5240c4bb782f5c26b241d0

### 1. 암호화 방식
- 비밀번호를 DB에 저장할 때 Hash를 이용했는데, Hash는 단방향 암호화와 양방향 암호화 중 어떤 암호화 방식에 해당할까요?
  - Hash는 단반향 암호화 방식에 해당합니다.
  - 원본 데이터를 암호화한 뒤에는 복호화를 통해 원본 데이터로 되돌릴 수 없습니다.
- 비밀번호를 그냥 저장하지 않고 Hash 한 값을 저장 했을 때의 좋은 점은 무엇인가요?
  - 비밀번호가 유출되더라고 사용자의 정보를 보호할 수 있습니다.
  - 알 수 없는 값으로 이루어져있기 때문에 개인정보 보호에 도움이 됩니다.
### 2. 인증 방식
- JWT(Json Web Token)을 이용해 인증 기능을 했는데, 만약 Access Token이 노출되었을 경우 발생할 수 있는 문제점은 무엇일까요?
  - 'Access Token'이 노출되면 악의적인 사용자가 해당 토큰을 사용해 사용자의 권한을 행사할 수 있습니다.
  - 이는 개인 정보 유출 및 무단 액세스 등 문제를 일으킬 수 있습니다.
- 해당 문제점을 보완하기 위한 방법으로는 어떤 것이 있을까요?
  - 'Refresh Token'을 사용해 'Access Token'을 발급하던가, 'Access Token'의 유효기간을 짧게 설정해 자주 갱신하는 방법이 있습니다.
### 3. 인증과 인가
- 인증과 인가가 무엇인지 각각 설명해 주세요.
  - 인증(Authentication) : 서비스를 이용하려는 사용자가 인증된 신불을 가진 사람이 맞는지 검증하는 작업을 뜻합니다. 일반적으로, 신분증 검사 작업에 해당합니다.
  - 인가(Autorization) : 이미 인증된 사용자가 특정 리소스에 접근하거나 특정 작업을 수행할 수 있는 권한이 있는지 검증하는 작업을 뜻합니다. 놀이공원에서 자유 이용권을 소지하고 있는지 확인하는 단계라고 보면 좋습니다.
- 위 API 구현 명세에서 인증을 필요로 하는 API와 그렇지 않은 API의 차이가 뭐라고 생각하시나요?
  - 인증이 필요로 하는 API는 사용자의 권한이 필요로하는 작업이 있을 경우 인증이 필요하고,
  - 인증이 필요로 하지 않는 API는 사용자의 권한이 없어도 작업할 수 있는 경우 인증이 필요하지 않습니다.
- 아이템 생성, 수정 API는 인증을 필요로 하지 않는다고 했지만 사실은 어느 API보다도 인증이 필요한 API입니다. 왜 그럴까요?
  - 아이템 생성, 수정은 게임관리자가 관리를 해야합니다.
  - 일반 사용자가 생성, 수정이 가능하게 된다면 그건 게임 생태계가 망가지게 될 것입니다.
### 4. Http Status Code
- 과제를 진행하면서 사용한 Http Status Code를 모두 나열하고, 각각이 의미하는 것과 어떤 상황에 사용했는지 작성해 주세요.
  - 200 (성공) : 서버가 요청을 제대로 처리했다는 의미, API의 동작이 모두 정상적으로 수행된 후 response 를 할 경우 사용
  - 401 (권한 없음) : 사용자 인증이 필요함을 나타냅니다, 로그인이 필요한 요청 시 사용
  - 403 (Forbidden, 금지됨) : 서버가 요청을 거부하고 있음을 나타냅니다, requset 에 값을 입력하지 않을 경우 사용
  - 404 (Not Found, 찾을 수 없음) : 서버가 요청한 리소스를 찾을 수 없음을 나타냅니다, request 의 값이 DB 안에 없을 경우 사용
  - 409 (충돌) : 서버가 요청을 수행하는 중에 충돌이 발생했음을 나타냅니다, DB에 이미 존재하는 값을 추가할려고 할 경우 사용
  - 500 (내부 서버 오류) : 서버에 오류가 발생하여 요청을 수행할 수 없음을 나타냅니다, 에러 핸들러 미들웨어에 사용
### 5. 게임 경제
- 현재는 간편한 구현을 위해 캐릭터 테이블에 money라는 게임 머니 컬럼만 추가하였습니다.
  - 이렇게 되었을 때 어떠한 단점이 있을 수 있을까요?
    - 돈이 언제 어디서 사용했는지 어디서 들어왔는지 등 내역을 알 수 없는 문제가 있습니다.
    - 즉, 버그를 사용해서 생긴 돈인지 정상적으로 생긴 돈인지 알 수 없습니다.
  - 이렇게 하지 않고 다르게 구현할 수 있는 방법은 어떤 것이 있을까요?
    - money에 대한 히스토리 테이블을 생성해서 변경 내역을 로깅(Logging)하여 저장합니다.
- 아이템 구입 시에 가격을 클라이언트에서 입력하게 하면 어떠한 문제점이 있을 수 있을까요?
  - 아이템 가격을 사용자가 임의대로 설정한 뒤 구입할 수 있게 됩니다.
  - 치트오매틱으로 클라이언트를 변조해 말도안되는 경우를 만들어 낼 수 있습니다.
