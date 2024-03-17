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
+ ## HTTP 메서드
  + ### API URI 고민
    + 리소스의 의미
      + 회원을 등록하고 수정하고 조회하는게 리소스가 아니다!
      + 예) 미네랄을 캐라 -> <span style="color:red"><U>**미네랄**</U></span>이 리소스
      + <U>**회원이라는 개념 자체가 바로 리소스다.**</U>
    + 리소스를 어덯게 식별하는게 좋을까?
      + 회원을 등록하고 수정하고 조회하는 것을 모두 배제
      + <U>**회원이라는 리소스만 식별하면 된다. -> 회원 리소스를 URI에 매핑**</U>
  + ### 리소스와 행위를 분리 
    + URI는 리소스만 식별!
    + 리소스와 해당 리소소를 대상으로 하는 행위를 분리
      + 리소스 : 회원
      + 행위 : 조회, 등록, 삭제, 변경
    + 리소스는 명사, 행위는 동사
  + ### 메서드
    + #### 속성 : 
      + 안전(Safe Methods)
        + 호출해도 리소스를 변경하지 않는다.
        + 안전은 해당 리소스만 고려한다.
      + 멱등(Idempotent Methods)
        + f(f(x)) = f(x)
        + 한번 호출하든 두번 호출하든 100번 호출하든 결과가 똑같다.
        + 멱등 메서드
          + GET : 한번 조회하든, 두번 조회하는 같은 결과가 조회된다.
          + PUT : 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
          + DELETE : 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
        + 예외 메서드
          + POST : <span style="color:red"><U>**멱등이 아니다!**</U></span> 두번 호출하면 같은 결제가 중복해서 발생할 수 있다.
        + 활용 : 
          + 자동 복구 매커니즘
          + 서버가 TIMEOUT 등으로 정상 응답을 못주었을 때, 클라이언트가 같은 요청을 다시 해도되는가? 판단 근거
        + 예)
          + Q. 재요청 중간에 다른곳에서 리소스를 변경해버리면?
            + 사용자1 : GET -> username:A, age:20
            + 사용자2 : PUT -> username:A, age:30
            + 사용다1 : GET -> username:A, age:30 -> <span style="color:red"><U>**사용자2의 영향으로 바뀐 데이터 조회**</U></span>
          + A. <span style="color:yellowgreen"><U>**멱등은 외부 요인으로 중간에 리소스가 변경되는 것 까지 고려하지는 않는다.**</U></span>
      + 캐시가능(Cacheable Methods)
        + 응답 결과 리소스를 캐시해서 사용해도 되는가?
        + GET, HEAD, POST, PATCH 캐시 가능
        + 실제로는 GET, HEAD 정도로만 캐시로 사용
          + POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음
    + #### GET : 리소스 조회
      + 의미 : 
        + 리소스 조회
        + 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트릴)를 통해서 전달
        + 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않음
      + 순서 : 
        + 1. 리소스 조회 - 메시지 전달 
          + ![http-method-get-1.png](images/http-method-get-1.png)
        + 2. 리소스 조회 - 서버 도착 
          + ![http-method-get-2.png](images/http-method-get-2.png)
        + 3. 리소스 조회 - 응답 데이터
          + ![http-method-get-3.png](images/http-method-get-3.png)
    + #### POST : 요청 데이터 처리, 주로 등록에 사용
      + 의미 : 
        + 요청 데이터 처리
        + <U>**메시지 바디를 통해 서버로 요청 데이터 전달**</U>
        + 서버는 요청 데이터를 처리
          + 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
        + 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
      + 순서 : 
        + 1. 리소스 등록 - 메시지 전달
          + ![http-method-post-1.png](images/http-method-post-1.png)
        + 2. 리소스 등록 - 신규 리소스 생성
          + ![http-method-post-2.png](images/http-method-post-2.png)
        + 3. 리소스 등록 - 응답 데이터
          + ![http-method-post-3.png](images/http-method-post-3.png)
      + 스펙 : POST 메서드는 대상 리소스가 리소소의 고유 한 의미 체계에 따라 요청에 포함 된 표현을 처리하도록 요청 합니다.
      + 예)
        + HTML 양식에 입력 된 필드와 같은 데이터 블록을 데이터 처리 프로세스에 제공
          + 예) HTML FORM에 입력한 정보로 회원가입, 주문 등에서 사용
        + 게시판 뉴스 그룹, 메일링 리스트, 블로그 또는 유사한 기사 그룹에 메시지 게시
          + 예) 게시판 글쓰기, 댓글 달기
        + 서버가 아직 식별하지 않은 새 리소스 생성
          + 예) 신규 주문 생성
        + 기존 자원에 데이터 추가
          + 예) 한 문서 끝에 내용 추가하기
      + 정리 : <U>**이 리소스 URI에 POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야함 -> 정해진 것이 없다.**</U>
        + 1. 새 리소스 생성(등록)
          2. 요청 데이터 처리
          3. 다른 메서드를 처리하기 애매한 경우
    + #### PUT : 리소스를 (완전히) 대체, 해당 리소스가 없으면 생성 
      + 리소스를 대체 : 
        + 리소스가 있으면 대체
          + 1. 리소스가 있는 경우
            + ![http-method-put-already-1.png](images/http-method-put-already-1.png)
            + ![http-method-put-already-2.png](images/http-method-put-already-2.png)
        + 리소스가 없으면 생성
          + 2. 리소스가 없는 경우
            + ![http-method-put-none-1.png](images/http-method-put-none-1.png)
            + ![http-method-put-none-2.png](images/http-method-put-none-2.png) 
        + 리소스 대체 
          + 3. 리소스를 완전히 대체
            + ![http-method-put-all-change-1.png](images/http-method-put-all-change-1.png)
            + ![http-method-put-all-change-2.png](images/http-method-put-all-change-2.png)  
      + 중요! 클라이언트가 리소스를 식별
        + 클라이언트가 리소스 위치를 알고 URI 지정
        + POST와 차이점
    + #### PATCH : 리소스 부분 변경
      + 순서 : 
        + 리소스 부분 변경
          + ![http-method-patch-spot-change-1.png](images/http-method-patch-spot-change-1.png)
          + ![http-method-patch-spot-change-2.png](images/http-method-patch-spot-change-2.png)
    + #### DELETE : 리소스 삭제
      + 순서 : 
        + 리소스 삭제
          + ![http-method-delete-1.png](images/http-method-delete-1.png)
          + ![http-method-delete-2.png](images/http-method-delete-2.png)
    + #### HEAD : GET과 동일하지만 메시지 부분을 제외하고, 상태 줄과 헤더만 반환
    + #### OPTIONS : 대상 리소스에 대한 통신 가능 옵션(메서드)을 설명(주로 CORS에서 사용)
    + #### CONNECT : 대상 자원으로 식별되는 서버에 대한 터널을 설정
    + #### TRACE : 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행