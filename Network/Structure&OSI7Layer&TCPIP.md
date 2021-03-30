## 네트워크 구조

### 규모
- 회사나 학교 등의 집단 크기에 따라 구분 : 사용자, 대역폭으로 표현, 사용량에 따라 네트워크 구조 설계 

### 업종
- 공공기관, 제조, 금융, 게임 등의 업종에 따른 서비스 중요도에 따라 네트워크 구조 설계

### 통신 방식과 경로
- Server & Client, Peer to Peer 등의 대표적인 방식이 있다.

### 토폴로지
- Star, Ring, Mesh, Bus, Tree, Redundancy
    - Star : 여러 컴퓨터가 한 대의 중계기를 통해 통신. 가까이 있어도 돌아가게 됨.
    - Ring : 중계기를 통하지 않고 통신, 멀어질 경우 점점 오래걸릴 수 있다.
    - Mesh : 별 모양으로 각각의 개체끼리 최단거리의 연결망을 유지하는것, 커질수록 복잡도가 증가
    - Bus : 서버와 컴퓨터가 큰 중심 버스라인을 통해 통신하는것
    - Tree : 물리적인 네트워크 구성 시 제일 많이 쓰임. 라우터, 스위치, 중계기 등에서 각각의 하위 네트워크가 트리구조로 구성
    - Redundancy : 가용성을 극대화, 이중화,삼중화 등에 쓰인다.

### 예시
* 홈 네트워크
    - 인터넷-ISP-모뎀-공유기-컴퓨터
* 기업용 네트워크
    - ISP - 전용선 - 라우터 - 방화벽 - L3 백본, L4 로드밸런서(-DMZ) - L2 스위치 - 서버, 컴퓨터
* 클라우드 네트워크 - AWS 기준
    - 인터넷 - route53 - igw - vpc - ELB - auto Scaling - security group - EC2 
    

## OSI 7 Layer

### 정의
- 네트워크 프로토콜과 통신을 7 계층으로 표현

### 목적
- 프로토콜을 기능별로 나누고 계층 별로 구분
- 벤더간 호환성을 위한 표준 필요 -> 쉬운 접근성으로 기술의 발전

### 역사
- 1970년대 초 네트워크는 정부 또는 특정 벤더에서 독점 개발, 공개형 모델 필요
- 1970년대 말 ISO(국제 표준화 기구)에 의해 관리
- 1984년 ISO 7498 릴리즈

### OSI 7 Layer 모델
> Application 응용 서비스 HTTP(웹), SMTP(메일)   
> Presentation 인코딩 / 암호화 / 압축   
> Session TCP/IP 통신 연결을 수립 / 유지 / 중단    
> Transport TCP/UDP   
> Network IP 통신, 라우팅   
> Data Link 이더넷, 랜카드, Mac 통신, 에러검출/재전송   
> Physical 네트워크 하드웨어 전송 기술   

* 1 계층 - physical
    - 기능 : 장치와 통신 매체 사이의 비정형 데이터의 전송을 담당, 디지털 bit(0&1)를 전기, 무선 또는 광 신호로 변환
    전송되는 방법, 제어 신호, 기계적 속성 등을 정의. 케이블, 인터페이스, 허브, 리피터 등이 이에 속함
* 2 계층 - Data Link
    - 기능 : 동일 네트워크 내에서 데이터 전송, 링크를 통해서 연결을 설정하고 관리. 물리계층에서 발생할 수 있는 오류를 감지하고 수정
    IEEE 802 에서 정의, MAC(Media Access Control), LLC(Logical Link Control)
    모뎀, 스위치 등의 장비가 해당
* 3 계층 - Network
    - 기능 : 다른 네트워크로 데이터 전송, IP 주소로 통신
    출발지 IP에서 목적지 IP로 데이터 통신 시 중간에서 라우팅 처리
    데이터가 큰 경우 분할 및 전송 후 목적지에서 재 조립하여 메세지 구현
    IP 통신과 라우팅, L3스위치, 라우터 해당
 * 4 계층 - Transport
    - 기능 : 호스트 간의 데이터(서비스) 전송. 오류 복구 및 흐름 제어, 완벽한 데이터 전송을 보장
    TCP / UDP, L4 계층을 특정 하드웨어로 구분하기가 모호
    Port를 제어한다는 의미로 L4 로드밸런서가 있다.
 * 5 계층 - Session
    - 기능 : 로컬 및 원격 애플리케이션 간의 IP / Port 연걸을 관리
    Session Table 화 해서 관리
 * 6 계층 - Presentation   
    - 기능 : 사용자 프로그램과 네트워크 형식 간에 데이터를 변환하여 표현과 독립성을 제공
    인코딩, 디코딩, 암호화, 압축, ASCII, JPG, MPEG
 * 7 계층 - Application
    - 기능 : 사용자와 가장 밀접한 소프트웨어
    이메일 서비스 SMTP, 또는 파일전송 FTP, HTTP 등

### TCP/IP

#### 정의
* 네트워크 프로토콜의 모음으로 패킷 통신 방식의 IP와 전송 조절 프로토콜인 TCP로 이루어져 있다.

#### 역사
* 1960년대 말 방위고등연구계획국(DARPA)이 연구
* 1990년대 네트워크 표준이 ISO 모델과 TCP/IP 모델로 좁혀짐
* 1990년대 말 TCP/IP 모델이 자주 쓰이면서 가장 일반적인 모델이 됨


#### TCP / IP 모델
> Application 응용 프로그램간 표준화 된 데이터 교환    
> Transport TCP / UDP   
> Network 패킷을 처리하고 다른 네트워크로 연결    
> Network Interface 물리 계층으로 네트워크 노드들을 상호 연결    

#### TCP/IP와 OSI 7 Layer 비교
> 1, 2 계층과 Network Interface : Ethernet      
> Network 계층 : IP, ICMP, OSPF      
> Transport 계층 : TCP, UDP   
> 5,6,7 계층과 Application 계층 : HTTP, SMTP, DNS   

### 캡슐화 

#### 인캡슐레이션 - 캡슐화, 위에서 아래로
> OSI 7 Layer----------------- Computer   
> Application----------------- Host Data           
> Presentation---------------- Host Data  
> Session--------------------- Data(Host Data)     
> Transpor(segment)----------- TCP header+Data    
> Network(Packet)------------- IP header+Data(TCP header + Data)   
> Data Link(Frame)------------ MAC LLC header + Data + FCS(오류검출)  
> Physical(bit)--------------- Signal (이진데이터, 상위 데이터들을 캡슐화해서 여기까지 내려옴) 

#### 디캡슐레이션 - 비캡슐화, 아래에서 위로
> OSI 7 Layer----------------- Computer   
> Application----------------- Host Data           
> Presentation---------------- Host Data  
> Session--------------------- Data(Host Data)     
> Transpor(segment)----------- TCP header+Data    
> Network(Packet)------------- IP header+Data(TCP header + Data)   
> Data Link(Frame)------------ MAC LLC header + Data + FCS(오류검출)  
> Physical(bit)--------------- Signal (이진데이터가 위로 올라가면서 사용한 데이터들이 줄어든다)


* 모든 데이터는 이러한 인캡슐레이션과 디캡슐레이션 과정을 통해 네트워크에서 통신된다.
