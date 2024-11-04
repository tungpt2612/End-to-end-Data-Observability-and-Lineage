# End to end Data Observability and Data Lineage

This section covers step by step guidance for PoX of Databand and Manta.

We have defined PoX as having 3 data stores: OLTP system, Staging, Data Warehouse. 

DataStage will be used for ETL from OLTP to Staging and Glue Job Spark will be used to ingesting data from Staging to Data Warehouse. Redshift Store Procedure will be used for further transform in Warehouse.

## Environments:
- Ubuntu 22.04
	- Host the OpenLineage / Marquez and Oracle Container Database
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
    

