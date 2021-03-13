 ### IPC 실습 (OS에서 개념은 익힘)

 #### IPC : InterProcess Communication
 1. file 사용
 2. Message Queue
 3. Shared Memory
 4. Pipe
 5. Signal
 6. Semaphore
 7. Socket


 #### pipe(파이프)
 * 기본 파이프는 단방향 통신
 * fork()로 자식 프로세스 만들었을 때, 부모와 자식간의 통신
 ```
char* msg = "Hello Child Process!";
int main() 
{
    char buf[255];
    int fd[2], pid, nbytes;
    if (pipe(fd) < 0) // pipe(fd) 로 파이프 생성, -1 이 나오면 생성이 안된것,  pipe생성은 커널영역
        exit(1);
    pid = fork(); // 이 함수 실행 다음 코드부터 부모/자식 프로세스로 나뉘어짐, pid가 달라지게 됨.
    if (pid> 0) {  // 부모 프로세스는 pid에 실제 프로세스 ID가 들어감
        write(fd[1], msg, MSGSIZE); // fd[1]에 씁니다. MSGSIZE는 msg의 데이터 사이즈
        exit(0);
    }
    else { // 자식 프로세스는 pid가 0이 들어감
        nbytes = read(fd[0], buf, MSGSIZE); // fd[0]으로 읽음 - 자식프로세스에서 fd주소가 달라짐, buf에 msg가 넘어옴 fd[0]으로 부모프로세스의 fd[1]을 읽을수있다
        printf("%d %s\n", nbytes, buf);
        exit(0);
    }
    return 0;
}
```

#### message queue
- 큐니까, 기본은 FIFO 정책으로 데이터 전송
- 메제지 큐는 부모/자식이 아니라 어느 프로세스간에라도 데이터 송수신이 가능
- 먼저 넣은 데이터가 먼저 읽혀진다 (큐)

```
msqid = msgget(key, msgflg) // key는 1234, msgflg는 옵션 - 메세지 큐를 하나 만든다
```
* msgflg 설정
    * IPC_CREAT: 새로운 키면 식별자를 새로 생성, IPC_CREAT|접근권한
    * 예: IPC_CREAT|0644 -> rw-r--r--
```
msgsnd(msqid, &sbuf, buf_length, IPC_NOWAIT) // 메세지 큐에 데이터를 넣는다
```
 * msgflg 설정: 블록모드(0) / 비블록 모드(IPC_NOWAIT)
```
msqid = msgget(key, msgflg) // key는 동일하게 1234로 해야 해당 큐의 msgid를 얻을 수 있음 , key 는 1234로 msgflg는 IPC_CREAT|0644 로 전달 하는 식
```

```
ssize_t msgrcv(int msgid, void *msgp, size_t msgsz, long msgtyp, int msgflg)
msgrcv(msqid, &rbuf, MSGSZ, 1, 0) // msgrcv 예
```
* msgtyp 설정: 0이면 첫번째 메세지, 양수이면 타입이 일치하는 첫번째 메세지
* msgflg 설정: 블록 모드 (0) / 비블록 모드(IPC_NOWAIT)

* 메시지큐 수신 프로그램 일부 코드 예
```
msqid = msgget(1234, IPC_CREATE|0644) // 키는 동일하게 1234로 해야 해당 큐의 msgid를 얻을 수 있음
msgrcv(msqid, &rbuf, MSGSZ, 1, 0)
```


#### 참고: ftok()
* ftok() : 키 생성을 위한 함수
    * path 경로명의 inode 값과 숫자값(id)를 기반으로 키 생성
    * 경로 삭제 후 재생성시 inode 값이 달라지므로, 이전과는 다른 키값이 리턴
```
#include <sys/ipc.h>
key_t ftok(const char *path, int id);

// 예
key = ftok("keyfile", 1);
id = msgget(key, IPC_CREAT|0640);
```    

