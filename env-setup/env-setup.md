# Environment Setup

This section covers step by step guidance for Environment Setup.

## Docker Setup:
- Login to Ubuntu 22.04 system
- Using root
```shell
sudo su -
```

- Oracle Container 19c
	- Serve as data source for traditional RDBMS.
- DataStage NextGen
	- Use for exporting data from Oracle RDBMS to csv file in Amazon S3
- Amazon S3
	- Contain Staging data in CSV format.
- Glue Spark
	- Use for ingesting data in csv file from Amazon S3 to Amazon Redshift
- RedShift Serverless
	- Use for DataWarehouse data store with store procedure.