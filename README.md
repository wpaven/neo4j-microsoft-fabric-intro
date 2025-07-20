# neo4j-microsoft-fabric
This repository provides examples for integrating Neo4j AuraDB with Microsoft Fabric.  The first workbook here was inspired by the [partners repo](https://github.com/neo4j-partners/neo4j-microsoft-fabric).  I wanted to provide additional examples as well as context for those new to Neo4j and Fabric to simplify getting started. 

- **Neo4j Workbook.ipynb** demonstrates moving data from a Microsoft Fabric Lakehouse to a Neo4j AuraDB graph using spark.
- **Neo4jToLakehouse.ipynb** demonstrates using spark to pull the results of a cypher query from Neo4j AuraDB into a dataframe and then write that dataframe to a Lakehouse .csv file or table.

## Overview

Neo4j AuraDB is the world's leading fully managed, cloud-based graph database service.  You can try it for free for a [14 day trial here](https://neo4j.com/product/auradb/).

Microsoft Fabric is a unified, end-to-end, cloud-based data analytics platform that combines various services into a single SaaS (Software as a Service) offering. It's used to streamline data movement, transformation, integration and management for analytics workflows. You can sign up for [free 60 day trial](https://www.microsoft.com/en-us/microsoft-fabric). You have to use your organization's email. It does not allow personal accounts.

At the time of this writing, I used trial versions of both products. The trial version of Fabric did not come with CoPilot. 

## Getting Started
