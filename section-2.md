## 현업에서 자주 사용하는 Docker CLI 익히기

### 이미지 다운로드
```bash
docker pull nginx

# docker pull nginx:latest와 동일함. 
# :tag 로 특정 버전 다운로드.
# Dockerhub 에서 받는 것! github나 npm 같은 느낌
```
 

### 이미지 조회/삭제
```bash
docker image ls 

# 다운된 이미지 리스트 조회. 
# created는 이미지가 '만들어진' 날짜. 다운받은 날짜가 아님.
```
```bash
docker image rm [imageId] 

# 이미지 삭제. imageId는 이미지마다 고유 ID.
``` 
```bash
docker image rm -f [imageId] 

# '중단된' 컨테이너 에서 썼던 이미지를 삭제.
# 실행중인건 안됨.  
```
 
### 컨테이너 생성 / 조회 / 실행


```bash
docker create [options] [imageName]

# 해당 이미지로 컨테이너를 '생성'만 함. 실행하지않음.
# 만약 이미지가 없다면 pull 하지않아도 자동으로 다운로드함.

# options :
#    --name [이름] : 이름 지정. 없으면 자동으로 만듦
```
```bash
docker ps

# 실행 중인 컨테이너 조회. 
# options :
#    -a : 모든 컨테이너 조회.
```
```bash
docker start [containerName or containerId]

# 컨테이너 시작
```
```bash
docker run [options] [imageName] 

# create와 start를 합친것. 만들고 바로 실행함

# options : 
#     -d : 백그라운드 실행
#     --name [name] : 컨테이너 이름 지정
#     -p [3000:80] : 컨테이너 포트 80번과 내 호스트 포트 3000번을 연결함
``` 
  

### 컨테이너 중지 / 삭제
```bash
docker stop [options] [containerName or containerId] 

# like 시스템 종료
```
```bash
docker kill [options] [containerName or containerId] 

# like 강제 종료
```
```bash
docker rm [options] [containerName or containerId]  

# '중지된' 컨테이너 삭제

# options : 
#     -f : 실행 되고 있어도 삭제
```

 
   

### 컨테이너 로그 조회
```bash
docker logs [containerName or containerId] [options]

# options :
#     --tail 10 : 끝 10줄만 출력
#     -f : 실시간 출력
#     --tail 0 -f : 입력한 순간부터 실시간 출력
```

  
### 컨테이너 내부에 접속하기
```bash
docker exec [prefixOptions] [containerName or containerId] [suffixOptions]

# prefixOptions : 
#     -it : 내부에 실시간으로 접속하여 머무르기.
#           -it이 없으면 내부에서 명령어 한번 실행후 나옴

# suffixOptions : bash, sh 등.. 쉘 지정
```

### redis 접속해보기
```bash
#1. 

docker run -d -p 6379:6379 redis

# redis 컨테이너를 6379 포트에서 실행
```
```bash
#2.

docker exec -it redis bash

# bash 로 redis를 실행한 container로 접속
``` 
```bash
#3.

redis-cli

# redis가 실행 중인 컨테이너에서 redis '프로그램'에 접속

# 1. set 1 a => OK
# 2. get 1 => "a"
# 3. del 1 => (integer) 1
```
   
    