## 스케쥴링
### 배치 처리 시스템, 시분할 시스템, 멀티 태스킹 등과 관련된 방법

1. 배치 처리 시스템
    - 시간축에 따라 자동으로 다음 응용 프로그램이 이어서 실행될 수 있도록 하는 시스템
    - 단점 
        - 여러 프로그램을 순서대로 돌리다보면 실행이 오래 걸리는 프로그램을 동작시키는 동안 
        다음 프로그램은 계속 대기해야 한다.
        - 동시에 여러 작업이 불가능하다 (MP3 들으면서 문서작성을 하거나 할 수 없다)  
        - 여러 사용자가 동시에 하나의 컴퓨터를 쓸 때도 문제가 된다(다중 사용자)
2. 시분할 시스템 : 다중 사용자 지원, 컴퓨터 응답시간 최소화
   - 응용프로그램이 CPU를 점유하는 시간을 잘게 쪼개어 실행될 수 있도록 하는 시스템
   - 다중 사용자도 지원할 수 있다: 응답시간을 쪼개서 짧게 나누기 때문
3. 멀티 태스킹 : 단일 CPU에서 여러 응용프로그램 동시 실행처럼 보이게 함
   - 단일 CPU에서, 여러 응용 프로그램이 동시에 실행되는 것처럼 보이도록 하는 시스템
   - 가령 하나의 응용프로그램의 출력을 아주 짧은 시간동안 지속한다
    이 프로그램의 결과물이 지속되는 동안 다른 응용프로그램을 동작시킨다. 
   - 사용자 입장에서는 동시에 일어나는 것처럼 보이지만 실제로는 아주 짧은 기간동안 응용 프로그램이 계속 바뀌는 것
4. 멀티 태스킹과 멀티 프로세싱
   - 멀티 태스킹 : 단일 CPU
   - 멀티 프로세싱: 여러 CPU에 하나의 프로그램을 병렬로 실행해서 실행속도를 극대화 시키는 시스템

### 멀티 프로그래밍

#### 최대한 CPU를 일정 시간당 많이 활용하도록 하는 시스템
- 시간 대비 CPU 활용도를 높이자
- 응용 프로그램을 짧은 시간 안에 실행 완료를 시킬 수 있음

#### 응용 프로그램은 온전히 CPU를 쓰기보다 다른 작업을 중간에 필요로 하는 경우가 많다
- 응용 프로그램이 실행되다가 파일을 읽는다(저장매체 I/O) 
- 응용 프로그램이 실행되다가 프린팅을 한다.
```
#Include <unistd.h> 
#Include <sys/types.h>
#Include <sys/stat.h>
#Include <fcntl.h>

Int main()
{
    int fd:
    fd = open("data.txt", O_RDONLY); // 파일 호출 - 이 동안 CPU가 블럭된다
    if(fd == -1)
    {
        printf("Error: can not open file\n");
        /* 파일을 열지 못해 종료 */
        return 1;
    } else 
    {
        printf(*File opened and now close_\n*);
        close(fd);
        return ();    
    }
}
```
- 코드에서 보듯이 CPU가 파일을 읽고 다음 행위에서 파일을 읽은 결과에 대한 응답을 위해 기다린다면 자원이 낭비된다
- 따라서 해당 시간동안 다른 응용프로그램으로 CPU를 사용한다면 더욱 효율적이다
- 실제로는 매우매우 짧은 시간에 이 작업들이 이뤄져 '동시적'인 것으로 느껴진다

## 정리
```
시분할 시스템, 멀티 프로그래밍, 멀티 태스킹이 유사한 의모로 통용된다
```
### 핵심
- 여러 응용 프로그램을 실행을 가능토록 함
- 응용 프로그램이 동시에 실행되는 것 처럼 보이도록 함
- CPU를 효율적으로 사용하여 짧은 시간 안에 프로그램이 실행 완료 될 수 있도록 함
- 응답시간을 줄여서 다중 사용자를 지원