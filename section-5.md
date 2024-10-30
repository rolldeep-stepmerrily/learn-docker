## Docker Compose


### compose.yaml
```yaml
  services:
    [서비스이름]:
      container_name: [컨테이너 이름]
      image: [이미지 이름]
      ports:
        - [포트:포트]
```

### CLI
```bash
# bash

docker compose up -d -build

# compose.yaml 파일을 기준으로 컨테이너들을 실행

# options :
#   -d : 백그라운드 실행
#   -build : 새로 build
```

```bash
# bash

docker compose down

# compose.yaml 파일을 기준으로 컨테이너들을 종료
```

```bash
# bash

docker compose logs

# 컨테이너들의 로그를 한번에 확인
```

```bash
# bash

docker compose pull

# 이미지를 docker hub에서 가져와서 업데이트
```