# (Optional) Local Spark

This section covers step by step guidance for using local spark in Ubuntu system.

## Setup Spark:
- Create Spark path
```console
mkdir -p ~/pyspark/
cd ~/pyspark/
```

- Download Jars file
```console
wget https://repo.maven.apache.org/maven2/io/openlineage/openlineage-spark/1.4.1/openlineage-spark-1.4.1.jar
```

- Create Python script
```console
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("OpenLineageExample") \
    .config("spark.jars.packages", "io.openlineage:openlineage-spark:1.4.1") \
    .config("spark.extraListeners", "io.openlineage.spark.agent.OpenLineageSparkListener") \
    .config('spark.openlineage.transport.type', 'http') \
    .config('spark.openlineage.transport.url', 'http://130.198.22.73:5000') \
    .config('spark.openlineage.transport.endpoint', '/api/v1/lineage') \
    .config('spark.openlineage.namespace', 'spark-integration-tungpt') \
    .getOrCreate()

spark.sparkContext.setLogLevel("INFO")

# Example PySpark job
df = spark.read.text("inputfile.txt")
df.show()

# Perform some transformation
transformed_df = df.selectExpr("length(value) as length")
transformed_df.show()

# Write data
transformed_df.write.mode('overwrite').csv("outputfile.csv")

spark.stop()
```

## Table of Contents

- The Data Product Solution Lifecycle
    - [The Lifecycle of Data Products](solutionarc/README.md)
- Create
    - [Overview](solutionarc/create/createREADME.md)
    - [Conceptual Architecture](solutionarc/create/createarc.md)
    - [IBM Product Architecture](solutionarc/create/createarcprod.md)
    - [Architectural Decisions and Considerations](solutionarc/create/createarcdec.md)
- Serve
    - [Overview](solutionarc/serve/servceREADME.md)
    - [Conceptual Architecture](solutionarc/serve/servearc.md)
    - [IBM Product Architecture](solutionarc/serve/servearcprod.md)
    - [Architectural Decisions and Considerations](solutionarc/serve/servearcdec.md)
- Realize
    - [Overview](solutionarc/realizeREADME.md)
    - [Conceptual Architecture](solutionarc/realize/realizearc.md)
    - [IBM Product Architecture](solutionarc/realize/realizearcprod.md)
    - [Architectural Decisions and Considerations](solutionarc/realize/realizearcdec.md)
- Deployment Architecture
    - [Overview](solutionarc/DARC/deparcoverviewREADME.md)
    - [Infrastructure Layer](solutionarc/DARC/deparcinfra.md)
    - [Software Layer](solutionarc/DARC/deparcsw.md)
    - [Application Layer](solutionarc/DARC/deparcapp.md)
- Use Cases
    - [Overview](solutionarc/UC/UCREADME.md)
    - [Use Case 1 - Banking](solutionarc/UC/UCBanking.md)
    - [Use Case 2 - API](solutionarc/UC/UCAPI.md)
    - [Use Case 3 - ESG](solutionarc/UC/UCESG.md)
    - [Use Case 4 - Retail](solutionarc/UC/UCRetail.md)
    - [Use Case 5 - Geospatial](solutionarc/UC/UCGeo.md)
    

