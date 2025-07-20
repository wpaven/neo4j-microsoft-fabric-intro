# neo4j-microsoft-fabric
This repository provides examples for integrating Neo4j AuraDB with Microsoft Fabric.  The first workbook here was inspired by the [partners repo](https://github.com/neo4j-partners/neo4j-microsoft-fabric).  I wanted to provide additional examples as well as context for those new to Neo4j and Fabric to simplify getting started. 

- **Neo4j Workbook.ipynb** demonstrates moving data from a Microsoft Fabric Lakehouse to a Neo4j AuraDB graph using spark.
- **Neo4jToLakehouse.ipynb** demonstrates using spark to pull the results of a cypher query from Neo4j AuraDB into a dataframe and then write that dataframe to a Lakehouse .csv file or table.
