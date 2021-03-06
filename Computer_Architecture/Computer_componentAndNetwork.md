## 컴퓨터 구성요소의 기능 및 이해

### 컴퓨터 구성요소의 인지와 기능의 조합
* 중앙 처리 장치
    - CPU/MPU(마이크로프로세서유닛)   
        - 마더보드(메인보드) : 데이터의 전달 통로가 디자인 되어있는 메인보드
        - CPU : 실행 프로그램의 명령 해석, 실행, 장치 제어, ALU, CU, 각종 레지스터로 구성
        - MPU : CPU를 LSI(고밀도 집적회로)화 한 일종의 통합장치
            - CISC, RISC, Bit Slice MPU 등이 존재한다.
    - 사물 인터넷 디바이스 H/W 플랫폼 종류
        - 아두이노(Arduino) : 2005년 이탈리아에서 탄생, 대표적인 오픈소스 H/W 플랫폼
        - Raspberry Pi, Galileo, Edison 등도 있음
* 주변장치(Peripheral Device)
    - 기억장치(Memory unit)
        - RAM (Random Access Memory) : 칠판같은 작업대. 정리하고 새로쓰고 하는 방식으로 데이터를 임시보관
            - DRAM(Dynamic RAM)
            - SRAM(Static RAM)
        - ROM (Read Only Memory) : 오직 읽을수만 있는 메모리로 영속성이있다.
    - 보조기억장치(Auxiliary memory device)
        - 하드 디스크, SSD, USB, SD카드 등 
        - 다량의 데이터 저장, 속도는 느리다
    - 주기억장치와 보조기억장치의 관계 : ROM은 CPU로부터 부팅시 자동으로 프로그램 실행 
      CPU는 메모리로부터 실행할 명령어와 데이터를 가져와서 처리, 디스크I/O는 메모리를 통해 처리.
        - 디스크의 성능과 파라미터
            1. 헤드를 해당 트랙으로 이동: 탐색 시간
            2. 데이터가 포함된 섹터가 회전되어 헤드 아래로 올 때까지 대기 : 회전 지연
            3. 데이터 전송 : 데이터 전송 시간
        - 디스크 접근 시간 = 탐색 시간 + 회전 지연 + 데이터 전송 시간
    - 입,츌력장치(Input/Output device)
        - 키보드, 마우스, 스캐너, 터치스크린, 바코드 판독기, 조이스틱 등
    

### 컴퓨터 구조와 통신
* 혁신적 네트워크의 발전에 따른 컴퓨터 구조의 변화
    * 비즈니스 환경에서의 통신과 네트워킹의 역할
    * 양자 컴퓨터
    * 웹의 진화 - 글로벌 인터넷
    
* 네트워크와 통신 추세
    * 80년대 이후 급속한 정보통신기술의 발전으로 컴퓨터 구조도 비약적으로 발전, 시장 구도의 변화.
    * Network Operating System 의 역할 확대
    * 네트워크 장비는 4차 산업혁명을 실현하기 위한 핵심 인프라
    * 최근 네트워크 장비가 미,중 무역전쟁의 핵심 쟁점으로 부각
    * 유,무선 통신, 방송 및 통신과 컴퓨터의 융합 등 컨버전스 가능
    * 고성능 컴퓨팅 기술을 활용한 대용량의 통신회선 교환이 가능
    * 소프트웨어 기술과 고밀도 집적 기술의 발달 -> 신형의 서비스와 장비 등장
    * 사용자 중심의 신기술이 활발하게 개발됨

* 양자 컴퓨터
    * 양자컴퓨터는 중첩, 얽힘 등 양자의 고유한 물리학적 특성을 이용하여, 다수의 정보를 동시 처리할 수 있는 새로운 개념의 컴퓨터
    * 현대 반도체 칩의 미세회로에서 발생하는 누설 전류로 인한 고전 컴퓨터 성능 한계 돌파를 위한 대안으로 양자 컴퓨터 필요성 대두
    * 양자컴퓨터는 양자적 정보 단위인 양자비트 또는 큐비트를 정보처리의 기본 단위로 하는 양자 병렬처리를 통해 정보처리 및 연산 속도가
    지수 함수적으로 증가하여 빠른 속도로 문제 해결이 가능.

* 웹 환경의 변화
    1세대 : www 바탕의 웹 디렉토리 검색을 이용한 컨텐츠 사용
    2세대 : 개방, 참여정신을 바탕으로 하는 쌍방향 웹기술(소셜네트워크, 위키 등)
    3세대 : 시맨틱 웹기술을 활용하여 웹페이지에 담긴 내용을 이해하고 개인형 맞춤 정보를 제공하는 지능형 웹 기술
    4세대(미래) : 현실과 가상이 연결되는 초 연결 지능화 웹 기술
  