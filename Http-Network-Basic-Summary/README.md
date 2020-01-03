# Http-Network-Basic-Summary

우에노 센 저자님의 '그림으로 배우는 Http&amp;Network Basic' 책 요약 내용입니다.


## [1장. 웹과 네트워크의 기본에 대해 알아보자.]

- HTTP 프로토콜은 세계 각지에서 지식을 공유하기 위해 생겨났다.

- 네트워크는 계층을 나누어 관리하는데, 그 이유는 특정 계층의 사양이 변경되었을 때 모든 프로토콜을 변경하지 않도록 하기 위함이다.

- 데이터를 전송하기 위해서는 각 계층에서 헤더를 붙이는 `캡슐화` 라는 작업을 진행한다.

- IP통신은 MAC주소를 가지고 통신을 하는데, MAC 주소는 네트워크 카드에 할당된 고유한 주소다. MAC주소는 `ARP(Address Resolution Protocol)`에 의해 `IP-to-MAC`으로 변환된다.

- TCP는 신뢰성있는 데이터를 전송에 대한 프로토콜이다. 신뢰를 위해 `3-way handshake`, `흐름제어`, `혼잡제어`와 같은 작업을 수행한다.

- `DNS(Domain Name Server)`는 도메인(URL)을 IP주소로 변환해주는 서비스를 제공하는 애플리케이션 계층에 속한다. 

- `URL(Uniform Resource Locator)`은 웹 페이지를 표시하기 위해 입력하는 주소를 의미한다.
  
- `URI(Uniform Resource Identifier)`는 리소스를 구별하는 식별자가 존재하는 문자열이다.
    - `ftp://ffp.is.co.za/rfc/rfc1808.txt`
    - `tel:+1-816-555-1212`
    - `news:comp.infosystems.www.servers.unix`
    - `http://www.ietf.org/rfc/rfc23969.txt` (URL)

## [2장. 간단한 프로토콜 HTTP]
