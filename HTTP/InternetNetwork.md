# Internet Network
<pre>
<b>인터넷 통신</b>
- 복잡한 인터넷 망에서 데이터를 보내려면 IP(Internet Protocol)가 필요하다.

<b>IP(Internet Protocol)</b>
- 송신 호스트와 수신 호스트가 패킷 교환 네트워크에서 <b>정보를 주고받는 데 사용하는 통신규약</b>
- 호스트간의 통신만을 담당
- 지정한 IP주소에 패킷(Packet)이라는 통신 단위로 데이터를 전달한다.

<b>패킷(Packet)</b>
- 송신측과 수신측 간에 데이터를 주고받을 때 네트워크를 통해서 전송되는 데이터 조각
- 패킷안에는 <b>출발지 IP, 목적지 IP, 기타...</b> 이 포함되어있다.
- pack + bucket

<b>전송 방법</b>
1. IP 패킷을 만든다.
2. 패킷 안에 있는 출발지, 목적지 IP 등을 인터넷으로 전달
3. 노드끼리 주소를 확인하여 목적지까지 정확하게 도달
4. 목적지에서 데이터를 받았을 경우 받았다는 OK 메시지를 왔던 노드를 기억하여 빠르게 답한다.

<b>IP 프로토콜의 한계(문제점)</b>
- <b>비연결성</b> : 패킷을 받을 서버가 없거나 서비스 불능 상태여도 패킷 전송이됨
- <b>비신뢰성</b> : 전송 중간에 패킷이 사라져도 알 수 없으며, 패킷이 순서대로 도착하지 않아도 해결 불가
- <b>프로그램 구분</b> : 같은 IP의 서버와 통신할 때, 애플리케이션이 둘 이상일 경우 포트가 없어서 구분 불가
</pre>
## TCP/UDP
<pre>
<b>TCP/IP 4 Layer</b>
- <b>L4 응용(애플리케이션) 계층</b> : TCP/UDP 기반의 응용 프로그램을 구현할 때 사용(HTTP, FTP, SSH)
- <b>L3 전송 계층</b> : 통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터 전송을 담당(TCP, UDP)
- <b>L2 인터넷 계층</b> : 통신 노드 간의 IP패킷을 전송하는 기능과 라우팅 기능 담당(IP, ARP, RARP)
- <b>L1 네트워크 엑세스 계층</b> : 물리적인 주소로 MAC을 사용(LAN 드라이버, 장비)

  <img src="https://github.com/RyuKyeongWoo/TIL/blob/main/HTTP/img/TCPIP4Layer.PNG"/>
  - <b>즉 데이터가 전송될 때 IP 패킷 안에 TCP 데이터, 그안에 전송 데이터를 포함하여 전송한다.</b>

  <img src="https://github.com/RyuKyeongWoo/TIL/blob/main/HTTP/img/TCPIP.PNG"/>
  - 노랑색 : IP 패킷, 초록색 : TCP 세그먼트
  - TCP/IP 패킷

<b>TCP</b>
- <b>전송 제어 프로토콜(Transmission Control Protocol)</b>
- TCP를 사용하면 IP 프로토콜의 한계를 극복할 수 있다.

- <b>연결 지향 : TCP 3 way handshake (가상 연결)</b>
  Server와 Client는 3-way handshaking 과정에서 SYN과 SYNACK을 주고 받으며, 양단간 세션 '상태'를 established한 '상태'로 만든다.
  세션 '상태'가  established가 되면 client와 server는 데이터를 주고 받을 수 있다.
  이렇게 TCP는 세션 '상태'에 따라 Server의 응답이 달라지는 Stateful 프로토콜이다.
  -> 나를 위한 전용 랜선이 보장된 것이 아닌 논리으로만 연결이 된것

  <img src="https://github.com/RyuKyeongWoo/TIL/blob/main/HTTP/img/TCP3WayHandshake.PNG"/>
  1. 클라이언트가 서버에게 연결요청(SYN)한다.
  2. 서버가 클라이언트에게 요청수락(ACK)과 동시에 연결요청(SYN)을 한다.
  3. 클라이언트가 마지막으로 요청수락(ACK)을 보낸다. -> 이때 데이터 전송을 같이 보내기도 함
  4. 세션 '상태'가  established가 되면 데이터 전송을 한다.

- <b>데이터 전달 보증</b>
  클라이언트가 서버에게 데이터를 전송하면 서버에서 잘 받았는지 응답해준다

- <b>데이터 순서 보장</b>
  클라이언트가 데이터를 보냈는데 서버에서 도착한 데이터의 순서가 다를 경우 전부 버리고 다시 요청하여 데이터를 받는다.

<b>TCP 안에 정보가 있기 때문에 가능(순서정보, 검증정보 등등) -> 신뢰할 수 있는 프로토콜</b>

<b>UDP</b>
- <b>사용자 데이터그램 프로토콜(User Datafram Protocol)</b>
- IP와 같은데 포트(PORT), 체크섬(네트워크를 통해 전달된 값이 변경되었는지를 검증) 정보가 추가된 프로토콜
- 데이터 전달 및 순서가 보장되지 않지만, 단순하고 3 way handshake가 없어서 빠르다.
</pre>
## PORT
<pre>
<b>PORT</b>
- 같은 IP 내에서 여러 애플리케이션과 통신을 해야할 때 포트를 이용해서 프로세스를 구분한다.

<b>규칙</b>
- 0 ~ 65535 : 할당 가능
- 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
  FTP : 20, 21
  TELNET - 23
  HTTP - 80
  HTTPS - 443

<b>IP와 PORT</b>
- IP로 목적지 서버를 찾고 그 서버 내에서 PROT로 애플리케이션을 구분한다.
- IP가 아파트이면 PORT는 동·호수
</pre>
## DNS
<pre>
<b>DNS</b>
- <b>도메인 네임 시스템 DNS(Domain Name System)</b>
- IP는 길고 어려워서 기억하기 어렵다.
- IP는 변경될 수 있다.
- 그러므로 <b>DNS를 통해서 도메인 명을 등록하고 쉽게 IP를 사용할 수 있다.</b>
- DNS는 도메인명을 IP 주소로 변환한다.
</pre>
## 정리
<pre>
- 복잡한 인터넷 망에서 데이터를 보내려면 IP가 있어야한다.
- 하지만 IP는 한계(문제점)이 있는데 그 문제점을 TCP가 해결해준다.
- UDP는 IP에 포트기능이 추가된 프로토콜이다.
- PORT는 같은 IP 안에서 동작하는 애플리케이션을 구분하기위해 사용한다.
- DNS는 IP는 변하기 쉽고 외우기 어려운데 도메인명을 등록해서 사용할 수 있도록 도와준다.
</pre>
