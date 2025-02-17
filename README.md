- Para entrar al sqlplus directo del contenedor con la base de datos:
docker exec -it oracle-21xe-oracledb-1 sqlplus sys/oracle@XEPDB1 as sysdba
- Para el admin:
docker exec -it oracle-21xe-admin-1 sqlplus sys/oracle@oracledb:1521/XEPDB1 as sysdba
