# (Optional) Local Spark

This section covers step by step guidance for using local spark in Ubuntu system.

## Setup Spark:
- Create Spark path
```console
mkdir -p ~/pyspark/
cd ~/pyspark/
```

- Download Jars file
```python
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

## Submit Spark Job
- Submit Job
```console
spark-submit --jars openlineage-spark-1.4.1.jar --files log4j-openlineage.xml openlineage_spark_tungpt.py
```

