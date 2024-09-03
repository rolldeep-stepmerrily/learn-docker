## 현업에서 자주 사용하는 Docker CLI 익히기

### 이미지 다운로드
1. docker pull nginx => docker pull nginx:latest와 동일함. 항상 최신버전을 다운로드받음 다른버전은 :version (태그명)
2. Dockerhub 에서 받는 것! => github이나 npm 같은 느낌

### 이미지 조회/삭제
1. docker image ls => 다운된 이미지 조회. created는 이미지가 '만들어진' 날짜. 다운받은 날짜가 아님. 
2. docker image rm [imageID]  => 이미지 삭제. imageID는 이미지마다 고유 ID.
3. docker image rm -f [imageID] => '중단된' 컨테이너 에서 썼던 이미지도 삭제가 안되는데 -f 하면 삭제됨. 실행중인건 안됨.
4. docker image rm $(docker images -q) => 컨테이너에서 사용하고 있지 않은 전체 이미지 삭제 (cmd 에서 안됨)
5. docker image rm -f $(docker images -q) => '중단된' 컨테이너에서 사용했던 이미지도 전체 삭제(cmd 에서 안됨)

### 컨테이너 생성 / 조회 / 실행
1. docker create nginx => 컨테이너를 '생성'만 함. 실행하지않음. 만약 이미지가 없다면 pull 하지않아도 자동으로 다운로드함
2. docker ps -a => 모든 컨테이너 조회. (그냥 ps는 실행중인것만)
3. docker start 컨테이너명 또는 ID (컨테이너 명은 미기입하면 랜덤 지정됨)
4. docker run [options] => create와 start를 합친것.
   1. -d : 백그라운드 실행
   2. --name [name] : 컨테이너 이름 지정
   3. -p [3000:80] : 컨테이너 포트 80번과 내 호스트 포트 3000번을 연결함
  

### 컨테이너 중지 / 삭제
1. docker stop [options] [컨테이너명 or ID] , docker kill [options] [컨테이너명 or ID] => 컨테이너 중지
   1. stop => 시스템 종료, kill => 강제 종료
2. docker rm [options] [컨테이너명 or ID] => '중지된' 컨테이너 삭제
   1. -f : 실행 되고 있어도 삭제
   2. docekr rm $(docekr ps -qa) : '중지된' 모든 컨테이너 삭제 (cmd 에서 안됨)
   

### 컨테이너 로그 조회
1. docker logs [컨테이너명 or ID] [options]
   1. --tail 10 : 끝 10줄만 출력
   2. -f : 실시간 출력
   3. --tail 0 -f : 입력한 순간부터 실시간 출력
  
### 컨테이너 내부에 접속하기
1. docker exec [prefix options] [컨테이너명 or ID] [suffix options]
   1. prefix options :
      1. -it : 내부에 실시간으로 접속하여 머무르기. 없다면 컨테이너 내부에서 명령어한번만 실행하고 빠져나옴. ex) docker exec 71c ls => 71c 아이디의 컨테이너에서 ls를 실행하고 다시 나옴
   2. suffix options :
      1. bash 등 쉘을 지정
2. redis 접속해보기
   1. docker run -d -p 6379:6379 redis
   2. docker exec -it reids bash
   3. redis-cli => redis가 띄워진 컨테이너에서, 다시 redis에 접속
      1. set 1 a => echo : OK
      2. get 1 => echo "a"
      3. del 1 => (integer) 1