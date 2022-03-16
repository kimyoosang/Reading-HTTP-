# Chapter5. 웹 서버

## **5.1 다채로운 웹 서버**

- 웹 서버는 HTTP 요청을 처리하고 응답을 제공한다. '웹 서버'라는 용어는 웹 서버 소프트웨어와 웹페이지 제공에 특화된 장비 양쪽 모두를 가리킨다
- 기능은 달라도, 모든 웹 서버는 리소스에 대한 HTTP 요청을 받아서 콘텐츠를 클라이언트에게 돌려준다

### **5.1.1 웹 서버 구현**

- 웹 서버는 HTTP 잋 그와 관련된 TCP 처리를 구현한 것이다. 웹 서버는 자신이 제공하는 리소스를 관리하고 웹 서버를 설정, 통제, 확장하기 위한 관리 기능을 제공한다
- 웹 서버는 HTTP 프로토콜을 구현하고, 웹 리소스를 관리하고, 웹 서버 관리 기능을 제공한다. 웹 서버는 TCP 커넥션 관리에 대한 책임을 운영체제와 나눠 갖는다. 운영체제는 컴퓨터 시스템의 하드웨어를 고나리하고 TCP/IP 네트워크 지원, 웹 리소스를 유지하기 위한 파일 시스템, 현재 연산 활동을 제어하기 이ㅜ한 프로세스 관리를 제공한다
- 웹 서버는 여러 가지 형태가 가능하다
  1. 다목적 소프투웨어 웹 서버를 표준 컴퓨터 시스템에 설치하고 실행할 수 있다
  2. 마이크로프로세서의 기적으로, 어떤 회사들은 사용자에게 판매할 전자기기 안에 몇 개의 컴퓨터 칩만으로 구현된 웹 서버를 내장시켜서 완전한 관리 콘솔로 제공한다

### **5.1.2 다목적 소프트웨어 웹 서버**

- 다목적 소프트웨어 웹 서버는 네트워크에 연결된 표준 컴퓨터 시스텥ㅁ에서 동작한다. 아파치나 W3C의 직소 같은 오픈 소스 소프트웨어를 사용할 수도 있고, 마이크로소프트나 아이플래닛의 웹 서버 같은 상용 소프트웨어를 사용할 수도 있다. 웹 서버 소프트웨어는 거의 모든 컴퓨터와 운영체제에서 동작한다

### **5.1.3 임베디드 웹 서버**

- 임베디드 웹 서버는 일반 소비자용 제품에 내장될 목적으로 만들어진 작은 웹 서버이다. 임베디드 웹 서버는 사용자가 그들의 일반 소비자용 기리를 간편한 웹 브라우저 인터페이스로 관리할 수 있게 해준다
- 아주 작은 임베디드 웹 서버의 예를 두 개 들면 다음과 같다
  1. 아이픽 성냥 머리 크기 웹 서버
  2. 넷미디어 사이트플레이어 SP1 이더넷 웹 서버

## **5.2 간단한 펄 웹 서버**

- 완전한 기능을 갖춘 HTTP 서버를 만들고자 한다면 해야 할 일이 좀 많다. 아파치 웹서버의 코어는 50,000줄이 넘는 코드로 되어 있고, 부가적인 처리 모듈들을 더하면 훨씬 커진다
- HTTP/1.1의 기능들을 지원하려면, 풍부한 리소스 지원, 가상 호스팅, 접근 제어, 로깅, 설정, 모니터링, 그 외의 성능을 위한 각종 기능들이 필요하다. 그러나 최소한으로 기능하는 HTTP 서버라면 30줄이하의 펄(Perl)코드로도 만들 수 있다

## **5.3 진짜 웹 서버가 하는 일**

- 최신식 상용 웹서버는 공통적으로 몇 가지 일들을 수행한다
  1. 커넥션을 맺는다
  - 클라이언트의 접속을 받아들이거나, 원치 않는 클라이언트라면 닫는다
  2. 요청을 받는다
  - HTTP 요청 메시지를 네트워크로부터 읽어 들인다
  3. 요청을 처리한다
  - 요청 메시지를 해석하고 행동을 취한다
  4. 리소스에 접근한다
  - 메시지에서 지정한 리소스에 접근한다
  5. 응답을 만든다
  - 올바른 헤더를 포함한 HTTP 응답 메시지를 생성한다
  6. 응답을 보낸다
  - 응답을 클라이언트에게 돌려준다
  7. 트랜잭션을 로그로 남긴다
  - 로그파일에 트랜잭션 완료에 대한 기록을 남긴다

## **5.4 단계 1: 클라이언트 커넥션 수락**

- 클라이언트가 이미 서버에 대해 열려있는 지속적 커넥션을 갖고 있다면, 클라이언트는 요청을 보내기 위해 그 커넥션을 사용할 수 있다. 그렇지 않다면, 클라이언트는 서버에 대한 새 커넥션을 열 필요가 있다

### **5.4.1 새 커넥션 다루기**

- 클라이언트가 웹 서버에 TCP 커넥션을 요청하면, 웹 서버는 그 커넥션을 맺고 TCP 커넥션에서 IP 주소를 추출하여 커넥션 맞은편에 어떤 클라이언트가 있는지 확인한다. 일단 새 커넥션이 맺어지고 받아들여지면, 서버는 새 커넥션을 커넥션 목록에 추가하고 커넥션에서 오가는 데이터를 지켜보기 위한 준비를 한다
- 웹 서버는 어떤 커넥션이든 마음대로 거절하거나 즉시 닫을 수 있다. 어떤 웹 서버들은 클라이언트의 IP주소나 호스트 명이 인가되지 않았거나 악의적이라고 알려진 것인 경우 커넥션을 닫느다. 다른 신원 식별 기법 또한 사용될 수 있다

### **5.4.2 클라이언트 호스트 명 식별**

- 대부분의 웹 서버는 '역 발향 DNS(reserve DNS)'를 사용해서 클라이언트의 IP 주소를 클라이언트의 호스트 명으로 변환하도록 설정되어 있다
- 웹 서버는 클라이언트 호스트명을 구체적인 접근 제어와 로깅을 위해 사용할 수 있다. 호스트 명 룩업(hostname lookup)은, 꽤 시간이 많이 걸릴 수 있어 웹 트랜잭션을 느려지게 할 수 있음을 미리 경고해두겠다. 많은 대용량 웹 서버는 호스트 명 분석(hostname resolution)을 꺼두거나 특정 콘텐츠에 대해서만 켜놓는다

### **5.4.3 ident를 통해 클라이언트 사용자 알아내기**

- 몇몇 웹 서버는 또한 IETF ident 프로토콜을 지원한다. ident 프로토콜은 서버에게 어떤 사용자 이름이 HTTP 커넥션을 초기화했는지 찾아낼 수 있게 해준다. 이 정보는 트깋 웹 서버 로깅에서 유용하기 때문에, 널리 쓰이는 일반 로그 포맷의 두 번째 필드는 각 HTTP 요청의 ident 사용자 이름을 담고 있다
- 만약 클라이언트가 identd 프로토콜을 지원한다면, 클라이언트는 ident 결과를 위해 TCP 포트 113번을 listen한다
- iden 프로토콜의 간단한 동작 순서는
  1. 클라이언트는 HTTP 커넥션을 연다
  2. 서버는 그 후 자신의 커넥션을 클라이언트의 ident 서버 포트를 향해 열고
  3. 새 커넥션에 대응하는 사용자 일므을 묻는 간단한 요청을 보낸다
- ident는 조직 내부에서는 잘 사용할 수 있지만 , 공공 인터넷에서는 다음을 포함한 여러 이유로 잘 동작하지 않는다
  1. 많은 클라이언트 PC는 identd 신원확인 프로토콜 데몬 소프트웨어를 실행하지 않는다
  2. ident 프로토콜은 HTTP 트랜잭션을 유의미하게 지연시킨다
  3. 방화벽이 ident 트래픽이 들어오는 것을 막는 경우가 많다
  4. ident 프로토콜은 안전하지 않고 조작하기 쉽다
  5. ident 프로토콜은 가상 IP 주소를 잘 지원하지 않는다
  6. 클라이언트 사용자 이름의 노출로 인한 프라이버시 침해의 우려가 있다