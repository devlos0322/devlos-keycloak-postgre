"# devlos-keycloak-postgre" 
Postgres DB 기반 Keycloak docker-compose file 입니다.
keycloak.dev.env 파일에서 관리자 계정 비밀번호 변경이 가능합니다.
db id, password를 임의로 변경시 문제가 발생하는 버그가 있습니다.

docker-compose up -d 실행시 접속되는 기본 포트는 38180 입니다.

Reference: https://medium.com/@gauthier.cassany/how-to-set-up-keycloak-with-docker-and-postgresql-b5236831ac29