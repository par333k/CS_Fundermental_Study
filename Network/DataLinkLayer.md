## 데이터 링크 계층

* 역할 : OSI 7 Layer의 2계층으로 인접한 네트워크 노드끼리 데이터를 전송하는 기능과 절차를 제공
물리계층에서 발생할 수 있는 오류를 감지하고 수정, 대표적인 프로토콜로 이더넷이 있으며 장비로는 스위치가있다.

* 구성 : 2개의 부 계층으로 구성
    - MAC : 물리적인 부분으로 매체간의 연결방식을 제어하고 1계층과 연결
        - MAC주소 : 명령어 cmd > ipconfig/all 또는 네트워크 설정에서 확인
        48bit(6 Bytes)로 6자리로 구성, 각 16진수로 표현
        앞에 3자리는 OUI 제조사 식별코드, 나머지 3자리는 제조사 내 일련변호.  
    - LLC : 논리적인 부분으로 Frame을 만들고 3계층과 연결
* 주요 기능
    - Framing : 데이터그램을 캡슐화하여 프레임단위로 만들고 헤더와 트레일러를 추가
    헤더는 목적지, 출발지 주고 그리고 데이터 내용을 정의
    트레일러는 비트 에러를 감지  
    - 회선 제어 : 신호간의 충돌이 발생하지 않도록 제어
    ENQ/ACK 방법, 전용 전송 링크 1:1, Polling 방법, 1: 다, Select 모드: 송신자가 나머지 수신자들을 선택하여 전송, 
    Poll 모드: 수신자에게 데이터 수신 여부를 확인하여 응답을 확인하고 전송 - multipoint  
* 흐름 제어
    - 송신자와 수신자의 데이터를 처리하는 속도 차이를 해결하기 위한 제어
    - Feedback 방식의 Flow Control이며 상위 계층은 Rate 기반
    - Stop & Wait
        - ACK를 기다리며 멈췄다 응답받고 전송
        - Frame을 재전송하게 되면 Duplicate frame 문제가 발생될 수 있음
        - Sequence number(1 bit)를 사용하여 동일 frame인지 구분하여 상위 계층으로 전달, 시퀀스 넘버를 확인하여 중복 데이터 확인
    - Sliding window
        - ACK 응답 없이 여러 개의 프레임이 연속으로 전송 가능
        - Window size는 전송과 수신측의 데이터가 저장되는 버퍼의 크기
        
* 오류 제어
    - 전송 중에 오류나 손실 발생 시 수신측은 에러를 탐지 및 재전송
        - ARQ : 프레임 손상 시 재전송이 수행되는 과정
            - Stop & Wait ARQ : 전송 측에서 NAK을 수신하면 재전송, 주어진 시간 내 ACK 안오면 재전송
            - Go Back n ARQ : 전송측 Frame 에서 문제된 Frame에 대한 손상을 받으면 거기서부터 다시 재전송
                - Selective Repeat ARQ : 손상된 Frame만 재전송

* 이더넷 프레임 구조 
    * Ethernet v2 : 데이터 링크 계층에서 MAC 통신과 프로토콜의 형식을 정의
        - Preamble 8byte, Dest Addr 6byte, Source Addr 6byte, Type 2byte, Data, FCS 4byte
            - Preamble : 이더넷 프레임의 시작과 동기화
            - Dest Addr : 목적지 MAC 주소, Src Addr: 출발지 MAC 주소
            - Type: 캡슐화 되어있는 패킷의 프로토콜 정의
            - Data: 상위 계층의 데이터로 46 ~ 1500 바이트의 크기, 46바이트보다 작으면 뒤에 패딩이 붙는다
            - FCS(Frame Check Sequence) : 에러체크


### 스위치와 ARP

* L2 스위치 
    - 정의 : 2계층의 대표적인 장비로 MAC 주소 기반 통신으로 허브의 단점을 보완(Half duplex -> Full duplex)
    1 Collision Domain -> 포트별 Collision Domain, 라우팅 기능이 있는 스위치는 L3 스위치라고도 부른다
    - 동작 방식 : 목적지 주소를 MAC 주소 테이블에서 확인하여 연결된 포트로 프레임 전달
        1. Learning : 출발지 주소가 MAC 주소 테이블에 없으면 해당 주소를 저장
        2. Flooding - Broadcasting : 목적지 주소가 MAC 주소 테이블에 없으면 전체 포트로 전달
        3. Forwarding : 목적지 주소가 MAC 주소 테이블에 있으면 해당 포트로 전달
        4. Filtering - Collision Domain : 출발지와 목적지가 같은 네트워크 영역이면 다른 네트워크로 전달하지 않음
        5. Aging : MAC 주소 테이블의 각 주소는 일정 시간 이후에 삭제
    - 정리 : 
        1. 프레임 유입
            - 신규: 출발지 주소 저장 -> Learning
            - 기존: Aging 타이머 재시작 -> Refresh
        2. 목적지가 Unknown -> Flooding
        3. 목적지가 MAC Table에 존재 -> Forwarding
        4. 목적지가 출발지와 같은 포트에 존재 -> Filtering
    
* ARP
    - 역할 : IP 주소를 통해서 MAC 주소를 알려주는 프로토콜, 컴퓨터 A가 컴퓨터 B에게 IP통신을 시도하고 통신을 수행하기 위해
    목적지 MAC 주소를 알아야 한다. 목적지 IP에 해당되는 MAC 주소를 알려주는 역할을 ARP가 해준다.
    - 동작 과정 : PC1 (172.20.10.1) - Switch - PC2(172.20.10.9) 일 경우
        1. PC1은 동일 네트워크 대역인 목적지 IP 172.20.10.9 로 패킷 전송을 시도, 목적지 MAC주소를 알기 위해서
        우선 자신의 ARP Cache Table을 확인
        2. PC1은 목적지 172.20.10.9에 대한 ARP Request 전송 - Broadcasting
        3. IP 172.20.10.9에서 목적지 MAC 주소를 ARP Reply로 전달
        4. 목적지 MAC 주소는 ARP Cache Table에 저장되고 패킷 전송
    - 헤더 구조 
        - Hardware Type : ARP가 동작하는 네트워크 환경, 이더넷
        - Protocol Type : 프로토콜 종류, 대부분 IPv4
        - Hardware & Protocol Length : MAC 주소 6Byte, IP주소 4Byte
        - Operation : 명령코드, 1 = ARP Request, 2 = ARP Reply
        - Hardware Address = MAC, Protocol Address = IP
    
### 스패닝트리 프로토콜

* Looping
    - 정의 : 같은 네트워크 대역 대에서 스위치에 연결된 경로가 2개 이상인 경우에 발생
    PC가 브로드캐스팅 패킷을 스위치들에게 전달하고 전달 받은 스위치들은 Flooding을 한다
    스위치들끼리 Flooding된 프레임이 서로 계속 전달되어 네트워크에 문제를 일으킨다.
    회선 및 스위치 이중화 또는 증축 등에 의해 발생
    물리적인 포트 연결의 실수 또는 잘못된 이중화 구성으로 L2에서 가장 빈번히 발생하는 이슈
    - 구조 : PC1-Switch1-Switch2,- Switch3
        1. PC1은 스위치 1에게 브로드캐스팅 전송
        2. 스위치1은 모든 포트에 브로드캐스팅 전송
        3. 전달받은 브로드캐스팅 프레임을 스위치 2, 3도 모든 포트에 전송
        4. 스위치 1은 스위치2, 3에게 다시 전달받은 브로드캐스팅을 다시 모든 포트에 전송
            - 브로드 캐스트 스톰

* STP : 스패닝 트리 프로토콜, 자동으로 루핑을 막아주는 알고리즘
    - 스패닝 트리 알고리즘으로도 불림. IEEE 802.1d 에 정의. STP는 2가지 개념을 가지고 있다.
        1. Bridge ID : 스위치는 우선순위로 0 ~ 65535로 설정, 낮을수록 우선순위가 높다.
        2. Path Cost : 링크의 속도(대역폭), 1000/링크 속도로 계산되며 작을수록 우선순위가 높다
        1Gbps 속도가 나오면서 계산법이 적합하지 않아 IEEE에서 각 대역폭 별 숫자 정의
    
    - STP의 요소 : 3개의 스위치 1,2,3이 있을 때
        1. Root Bridge: 네트워크당 1개 선출
            - 루트 브릿지 선출 : 서로 BPDU를 교환하고 가장 낮은 숫자가 루트 브릿지가 된다
        2. Root Port: Root Bridge가 아닌 스위치들은 1개 포트 선출
            - 루트 포트 선출 : 나머지 스위치들은 루트 브리지와 가장 빠르게 연결되는 루트 포트를 선출한다
        3. Designated Port: 각 세그먼트별 1개 포트 지정
            - 각 세그먼트별 루트 브리지와 가장 빠르게 연결되는 포트를 Designated 포트로 선출
            - 우선순위는 루트 브리지 ID > path cost > 브리지 ID > 포트 ID
    - BPDU : 스패닝 트리 프로토콜에 의해 스위치간 서로 주고받는 제어 프레임
        1. Configuration BPDU : 구성관련   
           Root BID - 루트 브리지로 선출될 스위치 정보
           Path Cost - 루트 브리지까지의 경로 비용
              - 루트 포트는 가장 낮은 루트 패스 코스트 값을 가진다.
           Bridge ID, Port ID - 나머지 스위치와 포트의 우선순위   
        2. TCN BPDU : 네트워크 내 구성 변경시 통보   
            우선순위 - 낮은 숫자가 더 높은 Priority를 가진다
                - 누가 더 작은 Root BID?   
                - 루트 브리지까지 더 낮은 Path Cost   
                - 연결된 스위치중 누가 더 낮은 BID?   
                - 연결된 포트중 누가 더 낮은 Port ID?   
           
    - 상태 변화 : 스위치의 포트는 스패닝 트리 프로토콜 안에서 5가지 상태로 표현된다
        1. Disabled
            - 포트가 Shut Down인 상태로 데이터 전송 불가. MAC 학습 불가, BPDU 송수신 불가
        2. Blocking
            - 부팅하거나 Disabled 상태를 Up 했을 때 첫번째 거치는 단계, BPDU만 송수신
        3. Listening - 15초
            - Blocking 포트가 루트 또는 데지그네이티드 포트로 선정되는 단계, BPDU만 송수신
        4. Learning - 15초
            - 리스닝 상태에서 특정 시간이 흐른 후 러닝 상태가 됨, MAC 학습 시작, BPDU만 송수신
        5. Forwarding
            - 러닝 상태에서 특정 시간이 흐른 후 포워딩 상태가 됨, 데이터 전송 시작, BPDU만 송수신
    
    - RSTP & MST
        - STP를 적용하면 포워딩 상태까지 30초~50초 걸림, 이 컨버전스 타임을 1-2초 내외로 단축
        - Learning & Listening 단계가 없음
    - MST
        - 네트워크 그룹이 많아지면 STP or RSTP BPDU 프레임이 많아지고 스위치 부하 발생
        - 여러개의 STP 그룹을 묶어서 효율적으로 관리
