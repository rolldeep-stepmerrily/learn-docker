## Dockerfile

Docker 이미지를 만들 때 사용 되는 참조되는 파일.  
특정 문법들을 익혀보자🤔


### JDK 17 베이스로 만들어 보기
```Docker
# Dockerfile

FROM openjdk:17-jdk # jdk-17을 베이스로 만들 것.
```
```bash
# bash

docker build -t [이미지명:태그명] [상대경로]

# options :
#   -t : 이름:태그 지정. 태그가 없다면 latest
```