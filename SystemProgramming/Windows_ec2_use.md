## 윈도우즈 ec2 우분투 접속

1. putty 설치

2. puttygen 으로 pem 파일을(aws 제공) ppk(private)로 변환 

3. putty에서 Auth에 ppk 파일 등록

4. putty session 에서 해당 aws 인스턴스의 퍼블릭 ip를 입력 (22번 포트, SSH, ubuntu@Ip)
    - time out 문제가 생길경우 vpc -> 라우팅테이블의 상태 확인, 넷게이트웨이 또는 인터넷게이트웨이 active인지 체크(blackhole이라 접속이 안되었음)

5. ubuntu로 로그인 하여 사용

      
