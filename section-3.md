## 도커 볼륨을 활용해 데이터 유실 방지하기

### 도커 볼륨 개념
* 컨테이너의 문제점
   1. 이 컨테이너에 띄워진 응용 프로그램이 계속 버전업이 되고 수정되는데, 이때 도커는 컨테이너에서 바뀔 부분을 수정하기보다, 새로 컨테이너를 만들어서 아예 교체하는식으로 작동함.
   2. A 컨테이너가 업데이트되면, 이미 업데이트된 B 컨테이너를 불러와서 갈아끼우는 식.
   3. 이에 따라 A 컨테이너에 저장되어있던 데이터가 사라질 수 있음
   4. 그래서 도커는 도커 볼륨을 사용함
   5. 애초에 데이터를 호스트 컴퓨터에 저장하고, 컨테이너는 호스트 컴퓨터에 접근해서 데이터를 사용함.
   
### 도커 볼륨 명령어
```bash
docker run -v [호스트의 디렉토리 절대경로]:[컨테이너의 디렉토리 절대경로] [이미지명]:[태그명]


# 주의점 :

# 1. 호스트의 디렉토리 절대 경로에 이미 디렉토리가 존재할 경우
# 호스트의 디렉토리가 컨테이너의 디렉토리를 덮어씀.

# 2. 호스트의 디렉토리 절대 경로에 이미 디렉토리가 없을 경우
# 컨테이너의 디렉토리에 있는 파일들을 호스트의 디렉토리로 복사함.
```


### MySQL 실행시켜보기

```bash
# 1.

docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=qwer1234 mysql

# mysql 컨테이너를 3306 포트에서 실행

# options :
#     -e : 환경변수 설정
```
```bash
# 2. 

docker exec -it [container] [shell]

# mysql 컨테이너 내부에 접속
```
```bash
# 3.

echo $MYSQL_ROOT_PASSWORD

# 환경변수 잘 설정되었는지 확인
```
```bash
# 4.

mysql -u root -p

# 이후에 비밀번호 입력하면 mysql 접근
```
```bash
# 5.

mysql> show databases; # db 확인
mysql> create database mydb; # db 생성
```
* 이후 컨테이너를 지우고 다시 만들면, 'mydb' db가 사라져있음. 
* 컨테이너 내부에 저장한 데이터가 컨테이너를 지우면서 사라졌기 때문
* 그래서 도커 볼륨을 사용해야한다.

### 볼륨을 활용해서 MySQL 실행시켜보기
```bash
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=qwer1234! -v [호스트 절대경로]:[컨테이너 절대경로] mysql


# windows 환경에서 경로 주의
#     ex) C:/docker/mysql/data:/var/lib/mysql

# 최초에 환경 변수가 볼륨에 저장되었기 때문에, 환경변수를 바꾸려면 mysql 내부에서 직접 바꿔야함

# options :
#     -v : 볼륨 생성(도커 볼륨 명령어 참고)

```

### PostgreSQL 도 실행시켜보기
```bash
docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=qwer1234! -v [호스트 절대경로]:[컨테이너 절대경로] --name postgres postgres
```
```bash
# 컨테이너에 접속한 뒤

psql -U postgres

postgres>\l # 데이터베이스 조회

postgres>\dn # 스키마 조회

postgres> create table public.test ( id SERIAL PRIMARY KEY, name VARCHAR(50)); # 테스트 테이블 생성

postgres> \dt public.* # 생성된 테이블 확인
```
* 컨테이너를 지웠다 새로 만들어도, 호스트에 데이터를 저장하기 때문에 유지된다.  

### MongoDB 도 실행시켜보기
```bash
docker run -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=qwer1234! -p 27017:27017 -v [호스트 절대경로]:[컨테이너 절대경로] -d mongo
```