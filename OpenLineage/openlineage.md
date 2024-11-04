# OpenLineage Setup

This section covers step by step guidance for OpenLineage Setup.

## Git Clone
- For current setup
```shell script
git clone https://github.com/OpenLineage/OpenLineage
```

- (Optional) For basic setup
```shell script
git clone git@github.com:MarquezProject/marquez.git && cd marquez
```

## Start container
```shell script
cd OpenLineage/integration/spark
docker-compose up
```

## Manually testing
- Simulate Start Event
```shell script
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
```shell script
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

<kbd>![marquez-console](/OpenLineage/marquez-console.png)<kbd>