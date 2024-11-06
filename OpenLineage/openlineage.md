# OpenLineage Setup

This section covers step by step guidance for OpenLineage Setup.

## Git Clone
- For current setup
```console
git clone https://github.com/OpenLineage/OpenLineage
```

- (Optional) For basic setup
```console
git clone git@github.com:MarquezProject/marquez.git && cd marquez
```

## Start container
```console
cd OpenLineage/integration/spark
docker-compose up
```

## Manually testing
- Simulate Start Event
```console
curl -X POST http://localhost:5000/api/v1/lineage   -i -H 'Content-Type: application/json'   -d '{
        "eventType": "START",
        "eventTime": "2020-12-28T19:52:00.001+10:00",
        "run": {
          "runId": "0176a8c2-fe01-7439-87e6-56a1a1b4029f"
        },
        "job": {
          "name": "my-job"
        },
        "inputs": [{
          "namespace": "my-namespace",
          "name": "my-input"
        }],  
        "producer": "https://github.com/OpenLineage/OpenLineage/blob/v1-0-0/client",
        "schemaURL": "https://openlineage.io/spec/1-0-5/OpenLineage.json#/definitions/RunEvent"
      }'
```

- Simulate Complete Event
```console
curl -X POST http://localhost:5000/api/v1/lineage   -i -H 'Content-Type: application/json'   -d '{
        "eventType": "COMPLETE",
        "eventTime": "2020-12-28T20:52:00.001+10:00",
        "run": {
          "runId": "0176a8c2-fe01-7439-87e6-56a1a1b4029f"
        },
        "job": {
          "namespace": "my-namespace",
          "name": "my-job"
        },
        "outputs": [{
          "namespace": "my-namespace",
          "name": "my-output",
          "facets": {
            "schema": {
              "_producer": "https://github.com/OpenLineage/OpenLineage/blob/v1-0-0/client",
              "_schemaURL": "https://github.com/OpenLineage/OpenLineage/blob/v1-0-0/spec/OpenLineage.json#/definitions/SchemaDatasetFacet",
              "fields": [
                { "name": "a", "type": "VARCHAR"},
                { "name": "b", "type": "VARCHAR"}
              ]
            }
          }
        }],     
        "producer": "https://github.com/OpenLineage/OpenLineage/blob/v1-0-0/client",
        "schemaURL": "https://openlineage.io/spec/1-0-5/OpenLineage.json#/definitions/RunEvent"
      }'
```

- Marquez Console

<kbd>![marquez-console](/OpenLineage/marquez-console-1.png)<kbd>
	
## Setup Gradle
Running Gradle change the port for listening Spark Job from 5000 to 8080

- Build
```console
cd OpenLineage
cd proxy/
cd backend/
./gradlew build
```

- Backup proxy file
```console
cp proxy.example.yml proxy.yml
```

- Run the gradle
```console
nohup ./gradlew runShadow &
```

- During Gradle running, it will receive Spark Events whenever the Spark Job runs. Spark Events will be spool as JSON format.
- Seperated those events into single json file and put it into folder have structure like this:
```console
cd input/openlineage/openlineage/events/
ls -ltr
```
```console
-rw-r--r--@ 1 tungpham  staff  6261 Oct 17 15:48 8.json
-rw-r--r--@ 1 tungpham  staff  6592 Oct 17 15:48 7.json
-rw-r--r--@ 1 tungpham  staff  6606 Oct 17 15:48 6.json
-rw-r--r--@ 1 tungpham  staff  6258 Oct 17 15:49 5.json
-rw-r--r--@ 1 tungpham  staff  5250 Oct 17 15:49 4.json
-rw-r--r--@ 1 tungpham  staff  5252 Oct 17 15:49 3.json
-rw-r--r--@ 1 tungpham  staff  5596 Oct 17 15:49 2.json
-rw-r--r--@ 1 tungpham  staff  5249 Oct 17 15:50 1.json
```

- Zip the file
```console
zip -r input.zip input
```
<kbd>![manta-input-structure](/OpenLineage/manta-input-structure.png)<kbd>
	
## <p align="center">[[Previous] Oracle Database](../oracle-database/oracle-db.md)</p>
## <p align="center">[[Next] Glue Job](../glue-job/glue-job.md)</p>
