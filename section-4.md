## Dockerfile

Docker 이미지를 만들 때 사용 되는 참조되는 파일.  
특정 문법들을 익혀보자🤔


### FROM
이미지의 베이스가 되는 이미지를 지정하는 명령어
```Docker
# Dockerfile 

# 기본 문법

FROM [이미지명]:[태그명]

# 태그명이 없다면 latest
```
```Docker
# Dockerfile

FROM node:18 # node 18 버전을 베이스로 만들 것.
```
```bash
# bash

docker build -t [이미지명:태그명] [상대경로]

# options :
#   -t : 이름:태그 지정. 태그가 없다면 latest
```
```bash
# bash 

docker run -d [만든이미지명]

# 만든 이미지로 컨테이너를 실행하면, Dockerfile 내용을 다 수행 한 뒤 꺼진다. 
```
```Docker
# Dockerfile

FROM node # 태그가 없으면 latest

ENTRYPOINT ["/bin/bash", "-c", "sleep 500"] # 바로 중단되는 것을 방지하기 위해 500초간 슬립

```

### COPY
호스트에서 컨테이너로 파일을 복사하는 명령어
```Docker
# Dockerfile

# 기본 문법

COPY [호스트경로] [컨테이너경로]
```

```Docker
# Dockerfile

FROM ubuntu

# 1. 파일 복사

COPY app.txt /app.txt 

# 2. 디렉토리 복사

COPY my-app /my-app/ # 디렉토리 복사할시 경로 주의할 것.

# 3. 와일드카드 활용

COPY  *.txt /text-files/ 
```


### .dockerignore 활용
```Plain Text
readme.txt
```
* 특정 파일을 컨테이너에 복사하지 않을 때 사용
* .gitignore와 같은 방식으로 동작함

### ENTRYPOINT 
컨테이너가 실행될 때 기본적으로 실행할 명령어를 지정하는 명령어
```Docker
# Dockerfile

# 기본 문법

ENTRYPOINT ["배열","요소로","띄어쓰기","구분"]
```
```Docker
# Dockerfile

FROM ubuntu

ENTRYPOINT ["/bin/bash","-c","echo hello"] 
```

### RUN
이미지의 생성 과정 중에 실핼할 명령어를 지정하는 명령어
```Docker
# Dockerfile

# 기본 문법

RUN [명령어]
```
```Docker
# Dockerfile

FROM ubuntu

RUN apt update && apt install -y git # git 설치

ENTRYPOINT ["/bin/bash","-c","sleep 500"]
```
### WORKDIR
컨테이너 내부에서 작업 디렉토리로 사용할 경로를 지정하는 명령어  
exec -it 으로 컨테이너 내부에 진입할 때 최초 진입 점이 되기도 함.
```Docker
# Dockerfile

# 기본 문법

WORKDIR [컨테이너 내부의 절대 경로]
```
```Docker
# Dockerfile

FROM ubuntu

WORKDIR /my-app # 컨테이너 내부에 my-app 라는 디렉토리로 작업 디렉토리 설정

COPY ./ ./

ENTRYPOINT ["/bin/bash","-c","sleep 500"]
```
### EXPOSE
컨테이너 내부에서 어떤 포트에 프로그램이 실행 되는지를 명시적으로 나타내는 명령어

* 이 명령어는작동에 영향을 끼치지 않는다.
* 실제 포트 할당은 docker run -p [포트번호]:[포트번호] 명령어로 기능함.

```Docker
# Dockerfile

# 기본 문법

EXPOSE [포트번호]
```