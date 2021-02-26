## 프로세스와 컨텍스트 스위칭

### 프로세스 구조
* 프로세스의 일반적 구성
    - text(CODE) : 프로그래밍 된 코드 자체를 저장하는 영역
    - data: 변수/초기화된 데이터
    - stack: 임시데이터(함수 호출, 로컬 변수 등)
    - heap: 코드에서 동적으로 만들어지는 데이터

* Stack 이란?
  - 컴파일 타임에 크기가 결정되므로 무한히 할당 불가
  - stack overflow 는 stack 영역의 메모리를 초과하면 발생
  - 함수 안에서 전언된 지역변수, 매개변수, 리턴 값, 돌아올 주소 등등이 저장

* PC(Program Counter) + SP(Stack Pointer)
    - PC: 명령어 주소 레지스터, 다음에 실행될 명령어의 주소를 갖고 있는 레지스터
    - SP: 스택에 데이터가 채워진 위치를 가리키는 레지스터
* EAX, EBP 레지스터
    - EBP: Extended Base Point Register 현재 스택의 가장 바닥을 가리키는 포인터.
    - EAX: Extended Accumlator Register 산술 논리의 연산에 사용되며 함수의 리턴값이 여기 저장된다

* Heap 이란?
    - 런타임시 크기가 결정되는 요소들이 저장되는 공간
    - 프로그램이 실행될 때 까지 알 수 없는 크기의 양만큼의 데이터를 저장하기 위해 프로그램의 프로세스가 사용할 수
    있는 미리 예약된 메모리 영역.
    - 동적으로 할당하여 활용하는 메모리영역  
```
#include <stdio.h>
#include <stdlib.h>
int main()
{
    int *data; // 포인터 변수
    data = (int *) malloc(sizeof(int)); // 동적 메모리의 주소를 리턴하는 부분, data 는 heap에 할당된다
    *data = 1;
    printf("%d\n", *data);
   
    return 0;
}
```
* Data
  -BSS : 초기화 되지 않은 전역 변수 (ex: int global_data1;)
  -DATA : 초기값이 있는 전역 변수 (ex: int global_data2 = 1;)


### 컨텍스트 스위칭
#### CPU에 실행할 프로세스를 교체하는 기술
1. 실행 중지할 프로세스 정보를 해당 프로세스의 PCB에 업데이트해서, 메인 메모리에 저장
2. 다음 실행할 프로세스 정보를 메인 메모리에 있는 해당 PCB정보(PC, SP)를 CPU에 넣고, 실행
> PC와 SP 값을 PCB라는 곳에 프로세스가 실행중인 상태를 캡쳐/구조화 해서 저장 
* Process Control Block (PCB)
1. Process ID
2. Register 값 (PC, SP 등)
3. Scheduling Info (Process State - ready, block, running..)
4. Memory Info (메모리사이즈 limit)  등

* PCB의 레지스터에 있는 PC, SP 값을 CPU의 실행레지스터에 적절하게 덮어씌워줌으로써 실행 컨텍스트를 변경한다
  - 실행 컨텍스트 변경에 대해 리눅스는 어셈블리어로 처리. 굉장히 빈번해서 매우 빠른 속도를 유지해야하고
    부하가 일어날 경우 오버헤드 위험이 있기 때문

* 초기 컴퓨터 프로그램들은 어셈블리어로 작성
  * 서로 다른 CPU 아키텍처가 등장할 때마다 매번 똑같은 프로그램 작성
  * 어셈블리어로는 프로그램 '작성' 속도가 매우 떨어짐. (동작 속도는 무척 빠름)
* 컴파일러 등장
  * CPU 아키텍쳐에 따라서는 컴파일러 프로그램만 만들면 됨, 기존 코드는 재작성할 필요 없음
  * 그러나, 어셈블리어로 작성한 코드보다는 속도가 떨어질 수 있음
  

