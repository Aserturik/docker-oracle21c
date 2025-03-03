Descarga la imagen de oracle: 
```
docker pull container-registry.oracle.com/database/express:21.3.0-xe
```

Clona mi repositorio:

```
git clone https://github.com/Aserturik/docker-oracle21c.git
# abre la carpeta
cd docker-oracle21c/
```

Ejecuta el docker-compose (para los contenedores administrador y base de datos):
```
docker-compose up --build
```

Mira las imagenes y los contenedores:
```
# Lista las imágenes:
docker images
Lista los contenedores en ejecución:
docker ps
# Lista los contenedores que no están en ejecución:
docker ps -a
```
### Inicia los contenedores si están detenidos:
```
docker start <nombre del contenedor>
# ejemplo: docker start oracle-21xe-admin-1 oracle-21xe-oracledb-1
```
#### Para entrar con sqlplus con rlwrap directo al contenedor con la base de datos:

- Opcion 1: ejecutar rlwrap con sqlplus (no se entra al contenedor)
```
# El nombre del contenedor puede cambiar por lo que debe ser el nombre que aparezca cuando se listan los contenedores (docker ps)

docker exec -it <contenedor administrador> sqlplus sys/oracle@oracledb:1521/XEPDB1 as sysdba
```

(Esto habilita rlwrap ya que estamos usando el contenedor administrador para conectarnos)

```
# ejemplo:
# docker exec -it oracle-21xe-admin-1 rlwrap sqlplus sys/oracle@oracledb:1521/XEPDB1 as sysdba
```

- Opción 2: entrar primero al contenedor administrador y después ejecutar sqlplus:

```
docker exec -it <contenedor administrador> bash
```

Ahora ejecutamos sqlplus:
```
sqlplus sys/oracle@oracledb:1521/XEPDB1 as sysdba
```

## Instalación del esquema de hr:

Repositorio de hr:
https://github.com/oracle-samples/db-sample-schemas/releases/tag/v21.1

Copiamos los scripts a el contenedor de  la base de datos:

```
docker cp Descargas/db-sample-schemas-21.1 oracle-21xe-oracledb-1:/home/oracle
```

Entramos al contenedor
docker exec -it oracle-21xe-oracledb-1 bash

```
# ve a /home/oracle/db-sample-schemas-21.1/human_resources

cd /home/oracle/db-sample-schemas-21.1/human_resources

# Ejecutamos el siguiente comando para cambiar las rutas de hr dentro del script de instalación:
sed -i.bak "s#@__SUB__CWD__/human_resources#@$(pwd)#g" *.sql

# Nos conectamos como sys:
sqlplus sys/oracle@XEPDB1 as sysdba
```

Instalación:

@hr_main.sql

specify password for HR as parameter 1:
Enter value for 1: oracle

specify default tablespeace for HR as parameter 2:
Enter value for 2: hr

specify temporary tablespace for HR as parameter 3:
Enter value for 3: temp

specify password for SYS as parameter 4:
Enter value for 4: oracle

specify log path as parameter 5: Enter value for 5: 
```
$ORACLE_HOME/demo/schema/log
```
specify connect string as parameter 6: Enter value for 6: 
```
localhost:1521/XEPDB
```
