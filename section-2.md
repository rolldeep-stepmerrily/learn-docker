## 현업에서 자주 사용하는 Docker CLI 익히기

### 이미지 다운로드
1. docker pull nginx => docker pull nginx:latest와 동일함. 항상 최신버전을 다운로드받음 다른버전은 :version (태그명)
2. Dockerhub 에서 받는 것! => github이나 npm 같은 느낌

### 이미지 조회/삭제
1. docker image ls => 다운된 이미지 조회. created는 이미지가 '만들어진' 날짜. 다운받은 날짜가 아님. 
2. docker image rm [imageID]  => 이미지 삭제. imageID는 이미지마다 고유 ID.
3. docker image rm -f [imageID] => '중단된' 컨테이너 에서 썼던 이미지도 삭제가 안되는데 -f 하면 삭제됨. 실행중인건 안됨.
4. docker image rm $(docker images -q) => 컨테이너에서 사용하고 있지 않은 전체 이미지 삭제
5. docker image rm -f $(docker images -q) => '중단된' 컨테이너에서 사용했던 이미지도 전체 삭제
