"# devlos-keycloak-postgre" 
Postgres DB 기반 Keycloak docker-compose file 입니다.
keycloak.dev.env 파일에서 관리자 계정 비밀번호 변경이 가능합니다.
db id, password를 임의로 변경시 문제가 발생하는 버그가 있습니다.

docker-compose up -d 실행시 접속되는 기본 포트는 38180 입니다.


## HTTP 프로토콜 사용
---
Keycloak은 기본적으로 HTTPS(SSL)을 사용하도록 설정되어 있어요. \
인증 서버는 외부로 노출되지않고 내부에서 동작하기 때문에, <b>HTTPS 설정을 해제</b>하려고 시도했어요. 
https://hub.docker.com/r/jboss/keycloak/ 에서 환경변수 정보를 찾을 수 없어, \
컨테이너 생성 후 직접 접속하여 SSL을 해제하는 식으로 사용했어요.

```bash
# 1. 컨테이너 ID 확인
docker container ls | grep keycloak

# 2. 컨테이너 접속
docker exec -it {contaierID} bash

# 3. 자격획득
cd opt/jboss/keycloak/bin
./kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin --password password

# 4. 접속 설정 변경
./kcadm.sh update realms/master -s sslRequired=NONE
```

## 이슈
---
다음과 같은 방법으로 컨테이너 생성시 자동으로 설정이 되도록 하려고 시도했는데, Keycloak 로딩 시점 문제인지 적용이 잘 되지 않고 있어요. 방법을 아시는 분은 말씀 부탁드려요 :)

```yaml 
    # docker-compose 파일에서 시도한 방법
    entrypoint: ["/bin/bash","-c"]
    command: [
      /opt/jboss/keycloak/bin/kcadm.sh config credentials --server http://localhost:8080/auth --realm master --user admin --password password %%
      /opt/jboss/keycloak/bin/kcadm.sh update realms/master -s sslRequired=NONE"]
```

Reference: https://medium.com/@gauthier.cassany/how-to-set-up-keycloak-with-docker-and-postgresql-b5236831ac29