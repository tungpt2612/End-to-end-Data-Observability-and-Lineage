# OpenLineage Setup

This section covers step by step guidance for AWS Glue Job Setup.

## Create Spark Job
- Create Spark Job on Amazon Glue
```python
# Install databand - run once
import os
os.system('pip install --upgrade pip')
os.system('pip install databand')


# Import pandas and databand libraries
#%additional_python_modules dbnd

import sys
import pandas as pd
from dbnd import dbnd_tracking, task, dataset_op_logger
import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("OpenLineageExample") \
    .config("spark.jars.packages", "io.openlineage:openlineage-spark:1.4.1") \
    .config("spark.extraListeners", "io.openlineage.spark.agent.OpenLineageSparkListener") \
    .config('spark.openlineage.transport.type', 'http') \
    .config('spark.openlineage.transport.url', 'http://130.198.22.73:8080') \
    .config('spark.openlineage.transport.endpoint', '/api/v1/lineage') \
    .config('spark.openlineage.namespace', 'glue-ingest-redshift') \
    .getOrCreate()

spark.sparkContext.setLogLevel("INFO")

spark._jsc.hadoopConfiguration().set("fs.s3a.access.key", "")
spark._jsc.hadoopConfiguration().set("fs.s3a.secret.key", "")
spark._jsc.hadoopConfiguration().set("fs.s3a.impl","org.apache.hadoop.fs.s3a.S3AFileSystem")
spark._jsc.hadoopConfiguration().set("com.amazonaws.services.s3.enableV4", "true")
spark._jsc.hadoopConfiguration().set("fs.s3a.aws.credentials.provider","org.apache.hadoop.fs.s3a.BasicAWSCredentialsProvider")
spark._jsc.hadoopConfiguration().set("fs.s3a.endpoint", "eu-west-3.amazonaws.com")

databand_url = 'https://ibm-sales-sandbox.databand.ai'
databand_access_token = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyODI5MTg0NSwianRpIjoiNzU3MDNiNmYtMzU5NC00MTc4LThkZGEtNWI1MDA3NWVmZjVmIiwidHlwZSI6ImFjY2VzcyIsImlkZW50aXR5IjoicGhhbS50aGFuaC50dW5nQGlibS5jb20iLCJuYmYiOjE3MjgyOTE4NDUsImV4cCI6MTc5MTM2Mzg0NSwidXNlcl9jbGFpbXMiOnsiZW52IjoiaWJtLXNhbGVzLXNhbmRib3gifX0.ixNFBDSdBcnxuFwtf0wlunOq3tcY6NqakyykKJJ_CE0'

# Provide a unique suffix that will be added to various assets tracked in Databand. We use this approach because
# in a workshop many users are running the same sample pipelines
unique_suffix = '_tpt'

@task
def read_raw_data():

    # Unique name for logging
    unique_file_name = "s3://ibmholbucket/stg_city.csv"

    # Log the data read
    with dataset_op_logger(unique_file_name,"read",with_schema=True,with_preview=True,with_stats=True,with_histograms=True,) as logger:
        df = spark.read.option("delimiter", ",").option("header", True).csv(unique_file_name)
        logger.set(data=df)
    return df

@task
def write_data(rawdata):

    unique_file_name_1 = 'jdbc:redshift://aws-workgroup.388505283736.ap-southeast-2.redshift-serverless.amazonaws.com:5439/dev/D_CITY'

    # Log writing the Camping Equipment csv
    with dataset_op_logger(unique_file_name_1, "write", with_schema=True,with_preview=True) as logger:
        # Write the csv file
        rawdata.write \
            .format("jdbc") \
            .option("url", "jdbc:redshift://aws-workgroup.388505283736.ap-southeast-2.redshift-serverless.amazonaws.com:5439/dev") \
            .option("dbtable", "D_CITY") \
            .option("user", "admin") \
            .option("password", "Redshift123") \
            .option("aws_iam_role", "arn:aws:iam::388505283736:role/service-role/AmazonRedshift-CommandsAccessRole-20241017T100123") \
            .option("tempdir", "s3://ibmholbucket/tmp/") \
            .mode("overwrite") \
            .save() 
        logger.set(data=rawdata)
        
# Call and track all steps in a pipeline

def redshift_ingesting():
    with dbnd_tracking(
            conf={
                "core": {
                    "databand_url": databand_url,
                    "databand_access_token": databand_access_token,
                }
            },
            job_name="redshift_ingesting" + unique_suffix,
            run_name="weekly",
            project_name="End-to-End Data Observability" + unique_suffix,
    ):
        # Call the step job - read data
        rawdata = read_raw_data()

        # Write data into Redshift
        write_data(rawdata)

# Invoke the main function
redshift_ingesting()                        

spark.stop()    

#    .config('spark.openlineage.transport.url', 'http://130.198.22.73:8080') \
```

- Config for detail configurations
<kbd>![glue-job-detail](/glue-job/glue-job-detail.png)<kbd>
	
- Libraries:
```console
Dependent JARs path
```

```console
s3://ibmholbucket/openlineage-spark-1.4.1.jar
```

- Job parameters 1:
```console
--additional-python-modules
```

```console
dbnd
```

- Job parameters 2:
```console
--conf
```

```console
spark.extraListeners=io.openlineage.spark.agent.OpenLineageSparkListener --conf spark.openlineage.transport.type=http --conf spark.openlineage.transport.url=http://130.198.22.73:8080 --conf spark.openlineage.transport.endpoint=/api/v1/lineage --conf spark.openlineage.transport.auth.type=api_key --conf spark.openlineage.transport.auth.apiKey=Ufc3konorJ8ObYeE5pu6w6jmMnf4wg7caAhGUnnu --conf spark.openlineage.namespace=glue-ingest-redshift
```

- Job parameters 3:
```console
--user-jars-first
```

```console
true
```

## Additional Jars for Glue Job
- Download Jar file from the link
```console
https://repo.maven.apache.org/maven2/io/openlineage/openlineage-spark/1.4.1/openlineage-spark-1.4.1.jar
```

- Upload Jar file to Amazon S3