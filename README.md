# http-study

+ ## 인터넷 네트워크
  + ### 인터넷 통신
  + ### IP(Internet Protocol)
    + 지정한 IP 주소(IP Address)에 데이터 전달
    + 패킷(Packet)이라는 통신 단위로 데이터 전달
    + 패킷
      + ![packet.png](images/packet.png)
    + IP 프로토콜의 한계
      + 비연결성
        + 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
          + ![packet-no-connection.png](images/packet-no-connection.png)
      + 비신뢰성
        + 중간에 패킷이 사라지면?
          + ![packet-disapper.png](images/packet-disapper.png)
        + 패킷이 순서대로 오지 않으면?
          + ![packet-no-order.png](images/packet-no-order.png)
      + 프로그램 구분
        + 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?
  + ### 인터넷 프로토콜 스택의 4계층
    + ![4-layers.png](images/4-layers.png)
    + ![protocol-layers.png](images/protocol-layers.png)
      + ### 1. 애플리케이션 계층 - HTTP, FTP
        +    
      + ### 2. 전송 계층 - TCP, UDP
        + TCP : 전송 제어 프로토콜 (Transmission Control Protocol)
          + 특징
            + 연결 지향 - TCP 3 way handshake (가상 연결)
              + 3 way handshake
                + ![tcp-3way-handshake.png](images/tcp-3way-handshake.png)
                + TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 에션을 수립하는 과정
            + 데이터 전달 보증
              + ![data-transfer.png](images/data-transfer.png)
            + 순서 보장
              + ![data-order.png](images/data-order.png)
            + 신뢰할 수 있는 프로토콜
            + 현재는 대부분 TCP 사용
          + TCP 세그먼트
            + 출발지 PORT
            + 목적지 PORT
            + 전송 제어
            + 순서
            + 검증 정보
        + UDP : 사용자 데이터그램 프로토콜 (User Datagram Protocol)
          + 특징
            + 하얀 도화지에 비유(기능이 거의 없음)
            + 연결 지향 - TCP 3 way handshake X
            + 데이터 전달보증 X
            + 순서 보장 X
            + 데이터 전달 및 순서가 보장되지 않지만, 단순하고 빠름
            + 정리
              + IP와 거의 같다. PORT 체크성 정도만 추가
              + 애플리케이션에서 추가 작업 필요
      + ### 3. 인터넷 계층 - IP
        + IP 패킷 정보 
          + 출발지 IP
          + 목적지 IP
      + ### 4. 네트워크 인터페이스 계층 