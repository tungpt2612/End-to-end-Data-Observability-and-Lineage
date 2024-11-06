## <p align="center">[[Previous] Environment Setup](../env-setup/env-setup.md)</p> 

# Oracle Database Setup

This section covers step by step guidance for Oracle Database Container Setup.

## Pull the image
- Using docker cmd
```console
docker pull doctorkirk/oracle-19c
```

## Setup the image
- Create folder
```console
mkdir -p /oracle_container/oracle-19c/oradata
```

- Navigate to the folder
```console
cd /oracle_container/
```

- Change owner
```console
sudo chown -R 54321:54321 oracle-19c/
```

- Run the image
```console
docker run --name oracle-19c -p 1521:1521 -e ORACLE_SID=corebank -e ORACLE_PWD=oracle -v /oracle_container/oracle-19c/oradata/:/opt/oracle/oradata doctorkirk/oracle-19c
```

## Setup the tables
- List the db container
```console
docker ps -a
```

```console
CONTAINER ID   IMAGE                                  COMMAND                  CREATED       STATUS                 PORTS                                                           NAMES
29633f615c23   doctorkirk/oracle-19c                  "/bin/sh -c 'exec $O…"   2 weeks ago   Up 2 weeks (healthy)   0.0.0.0:1521->1521/tcp, :::1521->1521/tcp                       oracle-19c
8b3544e7010a   jupyter/pyspark-notebook:spark-3.5.0   "tini -g -- start-no…"   2 weeks ago   Up 2 weeks (healthy)   4040/tcp, 0.0.0.0:8888->8888/tcp, :::8888->8888/tcp             spark_notebook_1
c7834a68d732   marquezproject/marquez-web             "/usr/src/app/entryp…"   2 weeks ago   Up 2 weeks             0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                       marquez-web
bfb1c41e626e   marquezproject/marquez                 "./wait-for-it.sh db…"   2 weeks ago   Up 2 weeks             0.0.0.0:5000-5001->5000-5001/tcp, :::5000-5001->5000-5001/tcp   marquez-api
73be337ad800   postgres:12.1                          "docker-entrypoint.s…"   2 weeks ago   Up 2 weeks             0.0.0.0:5432->5432/tcp, :::5432->5432/tcp                       marquez-db
```

- Log in to the container
```console
docker exec -it 29633f615c23 sh
```

- Login to oracle db
```console
sqlplus sys/oracle@corebank as sysdba
```

- Create user
```sql
create user temenos identified by oracle;
```

- Grant permissions
```sql
grant connect, dba, resource to temenos; 
```

- Login again as new user
```console
sqlplus temenos/oracle@corebank
```

- Create tables
```sql
CREATE TABLE "TEMENOS"."RAW_CITY" 
   (	"COUNTRY_CODE" VARCHAR2(20), 
	"CITY_CODE" VARCHAR2(20), 
	"CITY_NAME" VARCHAR2(20)
   );
```

```sql
CREATE TABLE "TEMENOS"."RAW_COUNTRY" 
   (	"COUNTRY_CODE" VARCHAR2(20), 
	"COUNTRY_NAME" VARCHAR2(20)
   );
```

- Insert records
```sql
INSERT INTO TEMENOS.RAW_CITY (COUNTRY_CODE,CITY_CODE,CITY_NAME) VALUES
	 ('vn','hn','HaNoi'),
	 ('my','kl','KualaLumpur');
```

```sql
INSERT INTO TEMENOS.RAW_COUNTRY (COUNTRY_CODE,COUNTRY_NAME) VALUES
	 ('vn','VietNam'),
	 ('my','Malaysia');
```

```sql
commit;
```

## <p align="center">[[Next] OpenLineage](../OpenLineage/openlineage.md)</p>