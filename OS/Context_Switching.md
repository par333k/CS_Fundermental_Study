## 프로세스와 컨텍스트 스위칭

### 프로세스 구조
* 프로세스의 일반적 구성
    - text(CODE) : 프로그래밍 된 코드 자체를 저장하는 영역
    - data: 변수/초기화된 데이터
    - stack: 임시데이터(함수 호출, 로컬 변수 등)
    - heap: 코드에서 동적으로 만들어지는 데이터

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

