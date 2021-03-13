## 프로세스 관리

### 프로그램, 프로세스, 스레드
* 프로그램: 바이너리, 코드 이미지, 응용 프로그램, Application, 또는 실행 파일
* 프로세스: 실행 중인 프로그램 (메모리 적재+프로세스 상태 정보 포함)
* 스레드
    * 리눅스 프로세스는 기본 스레드 포함
    * 싱글스레드 프로세스: 기본 프로세스
    * 멀티스레드 프로세스: 여러 스레드 존재

### 프로세스 ID
* 프로세스 ID
    * pid, 각 프로세스는 해당 시점에 unique한 pid를 가짐
    * pid 최대 값은 32768
    * 부호형(signed) 16비트 정수값 사용 => 2의 15승
> sudo vi /proc/sys/kernel/pid_max 명령어로 확인 가능
* 최근 할당된 pid가 200이라면, 그 이후는 201,202... 식으로 할당
* 맥스값이 넘어가게되면 다시 1부터 시작해서 안쓰는 pid값을 할당

### 프로세스 계층
* 최초 프로세스: init 프로세스, pid 1
* init 프로세스는 운영체제가 생성
* 다른 프로세스는 또다른 프로세스로부터 생성
    * 부모 프로세스, 자식 프로세스
* ppid 값이 부모 프로세스의 pid를 뜻함
> init 프로세스로부터 프로세스를 계속 포크하고, 별도의 실행이미지를 주입

* ppid 값 확인해보기
> ps -ef  
> -e    시스템상의 모든 프로세스에 대한 정보 출력  
> -f    다음 목록 출력(UID, PID, PPID, CPU%, STIME, TTY, TIME, CMD)  

### 프로세스와 소유자(owner) 관리
* 리눅스 내부에서는 프로세스의 소유자(사용자)와 그룹을 UID/GID (정수)로 관리
* 사용자에 보여줄때에만 UID와 사용자이름 매핑 정보를 기반으로 사용자 이름으로 제공
> ps -ef  
> sudo vi /etc/passwd  
> sudo vi /etc/shadow  

* /etc/passwd 확인하기
    - 사용자명(아이디): root / ubuntu
    - 패스워드 : x / x
    - 사용자ID(UID) : 0 / 1000
    - 그룹ID : 0 / 1000
    - 사용자정보 : root / Ubuntu
    - 홈디렉토리 : /root / /home/ubuntu
    - 쉘환경 : /bin/bash / /bin/bash
    
### 프로세스 관리 관련 시스템콜
* 우분투 리눅스에 gcc 설치(+ vi 에디터/한글 설정)
> sudo apt-get install gcc  
> gcc --version  
> gcc -o test.c test

### 부모 프로세스와 자식 프로세스
* bash 프로세스가 실행 파일의 부모 프로세스인 예
> ./pidinfo  
> pid=3671  
> ppid=2868

> ps  
>  PID TTY     TIME   CMD  
> 2868 pts/0 00:00:00 bash   
> 3669 pts/0 00:00:00 ps


### 프로세스 기본 구조
* TEXT, DATA, BSS, HEAP, STACK

#### 프로세스 생성
* 기본 프로세스 생성 과정
    1. TEXT, DATA, BSS, HEAP, STACK의 공간을 생성
    2. 프로세스 이미지를 해당 공간에 업로드하고 실행 시작
* 프로세스 계층: 다른 프로세스는 또다른 프로세스로부터 생성
    * 자식 프로세스, 부모 프로세스
    
#### fork()와 exec() 시스템 콜
* fork() 시스템콜 : 복사
    * 새로운 프로세스 공간을 별도로 만들고, fork() 시스템콜을 호출한 프로세스(부모 프로세스) 공간을 모두 복사
        * 별도의 프로세스 공간을 만들고, 부모 프로세스 공간의 데이터를 그대로 복사

* exec() 시스템콜 : 덮어씌움
    * exec() 시스템콜을 호출한 현재 프로세스 공간의 TEXT,DATA,BSS 영역을 새로운 프로세스의 이미지로 덮어씌움
        * 별도의 프로세스 공간을 만들지 않음

> fork()는 부모프로세스가 살아있고 exec() 는 부모프로세스가 사실상 사라진다

* fork() 시스템콜
> 헤더 파일: <unistd.h>  
> 함수 원형: pid_t fork(void); <- 인자가 없다, 통복사라서.  pid_t를 return 한다.  

``` 
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
int main()
{
    pid_t pid;
    printf("Before fork() call\n");
    pid = fork();
    // 여기서부터 프로세스 2개, 부모-자식 프로세스가됨
    if (pid == 0)
        printf("This is Child process. PID is %d\n", pid);
    else if (pid > 0)
        printf("this is Parent process. PID is %d\n", pid);
    else 
        printf("fork() is failed\n");
    return 0;        
}
```

* pid = fork()가 실행되면 부모 프로세스와 동일한 자식 프로세스가 별도 메모리 공간에 생성
* 자식 프로세스는 pid가 0으로 리턴, 부모 프로세스는 실제 pid리턴
* 두 프로세스의 변수 및 PC(Program Count) 값은 동일
* 새로운 프로세스 공간을 별도로 만들고, fork() 시스템콜을 호출한 프로세스(부모 프로세스) 공간을 모두 복사한 후,
fork() 시스템콜 이후 코드부터 실행
  
#### exec() 시스템콜 family
> 헤더 파일: <unistd.h>  
> 함수 원형:  
> int execl(const char *path, const char *arg, ...);  
> int execlp(const char *file, const char *arg, ...);  
> int execle(const char *path, const char *arg, ..., char * const envp[]);  
> int execv(const char *path, char *const argv[]);  
> int execvp(const char *file, char *const argv[]);  
> int execvpe(const char *file, char *const argv[], char *const envp[]);  

* execl() 시스템콜 예
```
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("execute ls\n");
    execl("/bin/ls", "ls", "-l", NULL);
    perror("execl is failed\n"); // 에러 출력  -> 이게 나오면 CODE 영역에 덮어씌워지는 것, 즉 execl 콜이 실패
    exit(1); // 에러 코드 전달
}
```

* execl()과 execlp() 시스템콜 사용법
> execl("디렉토리와 파일 이름이 합친 전체 이름", "명령어 인수 리스트", "끝은 NULL로 끝나야 함");

> // 파일 이름을 해당 프로세스를 실행한 프로세스의 환경변수(path)를 검색함  
> execp("파일 이름", "명령어 인수 리스트", "끝은 NULL로 끝나야 함");

* 명령어 인수 리스트
    * argv[0] = "ls"
    * argv[1] = "-al"
 
``` 
$ ls -al
execl("/bin/ls", "ls", "-al", NULL);
execlp("ls", "ls", "-al", NULL); => p가 해당 프로세스 실행한 프로세스의 환경변수를 검색(PATH -/bin)
```   

* execle() 시스템콜 사용법
``` 
// 환경 변수를 지정하고자 할 때
char *envp[] = { "USER=dave", "PATH=/bin", (char *)0 };
execle("ls", "ls", "-al", NULL, envp); // envp로 환경변수 설정을 매개변수에 전달
```

* execv(), execvp(), execve() 시스템 콜 사용법
``` 
// 인수 리스트를 내용으로 하는 문자열 배열
char *arg[] = { "ls", "-al", NULL };
execv("/bin/ls", arg);

// 파일 이름을 해당 프로세스를 실행한 프로세스의 환경변수(path)를 검색함
// 인수 리스트를 내용으로 하는 문자열 배열
char *arg[] = { "ls", "-al", NULL };
execvp("ls", arg);

// 환경변수를 지정하고자 할 때
char *envp[] = { "USER=dave", "PATH=/bin", (char *)0 };
// 인수 리스트를 내용으로 하는 문자열 배열
char *arg[] = { "ls", "-al", NULL };
execve("ls", arg);
```

* execl() 시스템콜 예
    * execl() 시스템콜을 실행시킨 프로세스 공간에 새로운 프로세스 이미지를 덮어씌우고, 새로운 프로세스를 실행
    * perror() 함수가 호출된다는 의미는 새로운 프로세스 이미지로 덮어씌우는 작업이 실행되지 못했다는 의미
        * 즉, execl() 시스템콜 실행 실패
``` 
int main() {
    // 에러가 나는 코드
    char *envp[] = { "USER=DAVE", NULL };
    char *arg[] = { "ls", "-al", NULL };
    
    printf("execute ls\n");
    execve("ls", arg, envp); // 환경변수 선언을 하지 않았기 때문에 에러
    perror("execl is failed\n");
    exit(1);
}
```

#### wait() 시스템콜
* wait() 함수를 사용하면, fork() 함수 호출시, 자식 프로세스가 종료할 때까지, 부모 프로세스가 기다림
* 자식 프로세스와 부모 프로세스의 동기화, 부모 프로세스가 자식 프로세스보다 먼저 죽는 경우를 막기 위해 사용(고아 프로세스)
  * 먼저 죽는 경우 고아프로세스, 또는 좀비프로세스라고 함
* 자식 프로세스가 종료되면 좀비 프로세스가 되어 해당 프로세스 조사를 위한 최소 정보만 가지고 있는 상태가 됨
* 완전히 끝나면, 해당 정보도 삭제되고, 부모 프로세스에 SIGCHLD 시그널이 보내짐  
  
``` 
int main() {
  int pid;
  int child_pid;
  int status;
  pid = fork(); // 새로운 프로세스공간(부모프로세스에 기초한)
  switch (pid) {
    case -1:
      perror("포크가 실패했다\n");
      break;
    case 0: //자식프로세스
       execl("/bin/ls", "ls", "-al", NULL); // ls 바이너리로 덮어씌우고 끝나면 부모프로세스에 시그널을보냄
       perror("execl 실패했다\n");
       break;
    default:  // 부모프로세스
       child_pid = wait(NULL); // 기다림 - (자식프로세스가 끝날때까지 시그널을 기다림)
       printf("ls가 완료됐다\n");
       printf(getpid(), child_pid);
       exit(0);     
  }
}
```

* wait() 리턴 값
    * 에러가 발생한 경우
```
#include <sys/wait.h>
pid_t wait (int *status) // status 자식 종료상태 값 exit(int status) exit는 종료상태값으로 자식프로세스를 종료한다
// 리턴값은 종료된 자식 프로세스의 pid
```

* status 정보를 통해 기본적인 자식 프로세스 관련 정보를 확인할 수 있음
    * 예 :
    ```
    int WIFEXITED(status); // 자식 프로세스가 정상 종료 시 리턴 값은 0이 아닌 값이 됨
    ```


#### fork(), execl(), wait() 시스템콜
* execl()만 사용하면, 부모 프로세스가 사라짐
* 이를 유지하기 위해, fork()로 새로운 프로세스 공간 복사 후, execl() 사용
* wait() 함수를 사용해서 부모 프로세스가 자식 프로세스가 끝날 때까지 기다릴 수 있음
> 쉘 프로그램은 fork(), exec()계열, wait() 함수를 기반으로 작성 가능


#### copy-on-write
* 기존 fork()는 통으로 프로세스를 복사하는데, 메모리 영역이 크다면 그만큼 부하도 크다
* 따라서 더 나은 속도로 프로세스를 복사하는 방식이 필요하다
* 자식프로세스 생성시, 부모 프로세스 페이지를 '우선'사용
* 부모 또는 자식 프로세스가 해당 페이지를 읽기가 아닌, 쓰기를 할 때
    * 이 때 페이지를 복사하고, 분리함
* 장점
    * 프로세스 생성 시간을 줄일 수 있음
    * 새로 생성된 프로세스에 새롭게 할당되어야 하는 페이지 수도 최소화.

* 읽을 때
    * 자식 프로세스 생성시, 부모 프로세스에서 사용하는 페이지를 그대로 사용(데이터 영역 자체는 같은 곳이고 어떤 포인터로 볼건지만 다른 개념)
* 쓸 때
    * 해당 요청 영역에 대한 실제 메모리에 저장된 데이터를 복사함, 이후 페이지 포인터를 변경하여 복사한 영역을 바라보게함

#### 프로세스 종료
* exit() 시스템콜: 프로세스 종료
```
#include <stdlib.h>
void exit(int status); // status - 프로세스 종료 상태번호
```
* main 함수의 return 0; 와 exit(0); 의 차이는?
    * exit() 함수: 즉시 프로세스를 종료함 (exit() 함수 다음에 있는 코드는 실행되지 않음)
    * return 0: 단지 main() 이라는 함수를 종료함
        * 단, main()에서 return 시, C언어 실행 파일에 기본으로 포함된 _start()함수를 호출하게 되고, 해당 함수는 결국 exit() 함수를 호출함
> main() 함수에서 return 0;은 exit() 호출과 큰 차이가 없음    

* 부모 프로세스는 status & 0377(비트연산) 계산 값으로 자식 프로세스 종료 상태 확인 가능
* 기본 사용 예
```
exit(EXIT_SUCCESS); // 0
exit(EXIT_FAILURE); // 1
```

#### exit() 시스템콜
* exit() 시스템콜 주요 동작
    * atexit() 에 등록된 함수 실행
    * 열려 있는 모든 입출력 스트림 버퍼 삭제 - ex) stdin, stdout, stderr 등 '파일'처럼 다뤄지는 것들
    * 프로세스가 오픈한 파일을 모두 닫음
    * tmpfile() 함수를 통해 생성한 임시 파일 삭제
        * 참고: tmpfile() - 임시 파일을 wb+ (쓸 수 있는 이진파일 형태) 모드로 오픈가능
        ```
        #include <stdio.h>
        FILE *tmpfile(void);
        ```
* atexit() 함수
    * 프로세스 종료시 실행될 함수를 등록하기 위해 사용
    * 등록된 함수를 등록된 역순서대로 실행

* atexit() 함수 예제
```
int main(void) {
    void exithandling(void);
    void goodbyemessage(void);
    int ret;

    ret = atexit(exithandling);
    if (ret != 0) perror("Error in atexit\n");
    ret = atexit(goodbyemessage);
    if (ret != 0) perror("Error in atexit\n");
    exit(EXIT_SUCCESS);
}

void exithandling(void) {
    printf("exit handling\n");
}

void goodbyemessage(void) {
    printf("see you again!\n");
}

```

#### nice() 시스템콜 : 우선순위 변경하기
* 프로세스 중 사실상 root가 소유한 프로세스만, 우선순위를 높일 수 있음
    * 다른 프로세스는 우선순위를 낮출 수만 있음
    * 스케쥴링방식에 따라 우선순위가 적용될수도 있고 안될 수도 있음

#### getpriority(), setpriority() 
```
#include <sys/resource.h>
int getpriority(int which, id_t who);
int setpriority(int which, id_t who, int value); // value 가 우선순위 값
```
* which: 프로세스(PRIO_PROCESS), 프로세스 그룹(PRIO_PGRP), 사용자(PRIO_USER) 별로 우선순위를 가져올 수 있음

* 예제 - priority.c(root 소유로 실행)
    * 이외에 스케쥴링 조작 시스템콜 기본 제공(POSIX 기반), 스케쥴링 알고리즘 변경으로 사용은 어려움
```
#include <sys/resource.h>
int which = PRIO_PROCESS;
id_t pid;
int ret; 
pid = getpid();
ret = getpriority(which, pid);
```
