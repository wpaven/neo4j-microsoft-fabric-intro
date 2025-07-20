# neo4j-microsoft-fabric
This repository provides examples for integrating Neo4j AuraDB with Microsoft Fabric.  The first workbook here was inspired by the [partners repo](https://github.com/neo4j-partners/neo4j-microsoft-fabric).  I wanted to provide additional examples as well as context for those new to Neo4j and Fabric to simplify getting started. 

- **Neo4j Workbook.ipynb** demonstrates moving data from a Microsoft Fabric Lakehouse to a Neo4j AuraDB graph using spark.
- **Neo4jToLakehouse.ipynb** demonstrates using spark to pull the results of a cypher query from Neo4j AuraDB into a dataframe and then write that dataframe to a Lakehouse .csv file or table.

## Overview

Neo4j AuraDB is the world's leading fully managed, cloud-based graph database service.  You can try it for free for a [14 day trial here](https://neo4j.com/product/auradb/).

Microsoft Fabric is a unified, end-to-end, cloud-based data analytics platform that combines various services into a single SaaS (Software as a Service) offering. It's used to streamline data movement, transformation, integration and management for analytics workflows. You can sign up for [free 60 day trial](https://www.microsoft.com/en-us/microsoft-fabric). You have to use your organization's email. It does not allow personal accounts.

At the time of this writing, I used trial versions of both products. The trial version of Fabric did not come with CoPilot. 

## Getting Started

To begin, we need to get some data into a Microsoft Fabic Lakehouse.
- Login to Fabric
- Select 'My workspace'
- Click 'New Item'
- Select Lakehouse
- In the modal that pops up, give the lakehouse a name. I named mine 'Lakehouse1'
- Click Create

Now you will see Lakehouse1 available in the left pane as a chiclet tile to be selected. Click the Lakehouse1 tile to load the lakehouse pane.

In this pane we see Lakehouse1 has 2 subfolders for Tables and Files.  Both of which are currently empty.  We want to create subfolders for our example data as well as the spark driver and additionally for any export from Neo4j.

- Click the 3 ellipsis to the right of the Files folder
- Select 'New subfolder'
- Name the folder 'Northwind'
- Click 'Create'

Repeat the above to create a folder named 'Drivers' and another named 'ExportFromNeo4j'

This creates the Northwind Folder.  In the Northwind folder we can load our Northwind data. The Northwind dataset is [available as csv files here](https://github.com/neo4j-graph-examples/northwind/tree/main/import).  To load the files:
- Click the 3 ellipsis to right of the Northwind Folder
- Select Upload
- Select Upload files
- Select the .csv files to be uploaded
- Click Upload

We now have our sample data in the Lakehouse.  
