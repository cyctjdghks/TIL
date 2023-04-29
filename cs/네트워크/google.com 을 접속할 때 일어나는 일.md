# https://www.google.com/ 을 접속할 때 일어나는 일

1. 도메인 이름 해석 (DNS resolution): 브라우저는 도메인 이름 시스템(DNS)을 사용하여 "www.google.com" 이라는 도메인 이름을 해당 서버의 IP 주소로 변환합니다.

2. TCP 연결 설정: 브라우저는 변환된 IP 주소로 서버와 통신하기 위해 TCP (Transmission Control Protocol) 연결을 설정합니다. 이 과정에서 3-way handshake가 이루어집니다.

3. SSL/TLS 암호화: "https://"로 시작하는 URL은 웹 페이지가 SSL/TLS 보안 프로토콜을 사용한다는 것을 나타냅니다. 브라우저와 서버 사이에서 SSL/TLS 핸드셰이크가 이루어져 암호화된 통신이 가능해집니다.

4. HTTP 요청: 브라우저는 웹 페이지의 내용을 요청하기 위해 HTTP(Hypertext Transfer Protocol) 요청을 서버에 보냅니다. 이 요청에는 URL, 브라우저 종류 등의 정보가 포함됩니다.

5. HTTP 응답: 서버는 요청된 웹 페이지의 HTML, CSS, JavaScript 및 이미지 등의 리소스를 HTTP 응답으로 전송합니다. 브라우저는 응답을 받아 리소스를 다운로드합니다.

6. 웹 페이지 렌더링: 브라우저는 받은 리소스를 해석하여 웹 페이지를 구성하고, 화면에 표시합니다. 이 과정에서 HTML, CSS, JavaScript 등의 코드가 실행되며, 웹 페이지의 최종 형태가 사용자에게 보여집니다.

- 시나리오
  - 사용자가 웹 브라우저를 엽니다.
  - 주소 창에 "https://www.google.com" 을 입력하고 Enter 키를 누릅니다.
  - 브라우저는 도메인 이름 시스템(DNS)을 사용하여 "www.google.com" 을 IP 주소로 변환합니다.
  - 브라우저는 해당 IP 주소로 TCP 연결을 설정하고 서버와 3-way handshake를 진행합니다.
  - 브라우저는 SSL/TLS 암호화를 사용하여 서버와 안전한 연결을 수립합니다.
  - 브라우저는 웹 페이지의 내용을 요청하기 위해 서버에 HTTP 요청 메시지를 보냅니다.
  - Google 서버는 요청에 대한 HTTP 응답을 생성하여 브라우저에 전송합니다. 이 응답에는 HTML, CSS, JavaScript 등의 웹 페이지 리소스가 포함됩니다.
  - 브라우저는 받은 리소스를 해석하고, 화면에 웹 페이지를 렌더링합니다.
  - 웹 브라우저에서 Google 웹 페이지를 볼 수 있습니다.
