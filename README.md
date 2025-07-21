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

We also require the Neo4j Spark Connector. You can [download the .jar file here](https://github.com/neo4j/neo4j-spark-connector/releases).  Upload the Neo4j Spark Connector. jar to the Drivers folder.  I used *neo4j-spark-connector-5.3.8-s_2.12.jar*

Finally, the Neo4j credentials are hardcoded in one example, and pulled from a *neo4j-conn.json* file in another.  Update the parameters in this file with the details for your Neo4j AuraDB instance and then upload this file to the Drivers folder as well. 

With our Lakehouse created and files uploaded, we're now ready to start moving data.

## Using the Workbooks 
Select Lakehouse1 from the tiles on the right pane.
Select 'Open Workbook' from the dropdown at the top
Select 'New Workbook'

You are now in a new Notebook.  Rename if from 'Notebook' by clicking the name of the Notebook to edit. Rename it 'Neo4j Workbook'

There is likely a way to import an existing workbook, but I haven't discovered it yet. I'm new to Fabric, so sharing what I've learned so far.  Once you have the noteobook, you can cut and paste from the cells here into blocks in the new notebook.  After running a block, at the bottom you'll find a '+ Code' appear on rollover.  Click this to add additional cells.  You'll want to create 2 notebooks and name similarily.  Download is readily apparent from the top menu items.  I believe import requires a separate workspace task, or I'm just missing it for some reason.

In the lower right of a cell, make sure 'Pyspark (Python)' is selected or you may see syntax errors.

### Neo4j Workbook.ipynb
This workbook will migrate the .csv files in the Northwind directory to a graph in Neo4j AuraDB.

I started with the one in the partners repo.  The first difference you'll note is the `%%configure` in the first cell to import the Neo4j Spark Connector .jar. Without this, you'll get misleading errors when trying to import the nodes. The error message will tell you something about not being able to parse the orderID, but it's really choking on line 11 in the 4th cell with the reference to `org.neo4j.spark.DataSource`  

In **cell 1**, you need to update the absolute path for the jar. To find the absolute path for your instance:
- Click the Lakehouse1 tile on the left panel
- Select the 'Drivers' folder and right-click on it
- Select properties
- In the properties pane you will find the ABFS path for the file.
- Copy this ABFS to the spark.jars value in the first cell

Note: Running the cell took 2-2.5 minutes for me to import the .jar into the session.  The rest of the cells run relatively quickly.

Also, you'll see the `%%spark`in each cell. This also differs from the original .ipynb but is required for running the spark jobs.

In **cell 2**, you will also want to set the `absfss_Base_Path` for your files finding the ABFS for the Northwind directory.

In **cell 4**, you need to set `neo4jUrl`, `neo4jUsername`, and `neo4jPassword` to the values for your Neo4j AuraDB instance.  If you want to import from *neo4j-conn.json*, see the example in *Neo4jToLakehouse.ipynb*.

Run these cells in order and you can then view your graph in the Query pane of your Neo4j AuraDB instance.

### Neo4j Workbook.ipynb
This workbook will migrate the results of a cypher query to Neo4j AuraDB to a .csv file and table in the Microsoft Fabric Lakehouse.

**cell 1** Update similar to cell 1 above to set ABFS for the Neo3j Spark Connector .jar file

**cell 2** This cell pulls the Neo4j Credentials from a file saved in the lakehouse.  Update the value of `absfss_JSON_Base_Path` with the ABFS for the *neo4j-conn.json* file. Make sure this file has been updated with the details for your Neo4j AuraDB instance.

**cell 3** Take a look at the results after running this block.  You can change the `query` as well as desired.

**cell 4** This block writes the results of the query above to a .csv file in the **ExportFromNeo4j** directory.  Make sure this diretory exists in your Lakehouse.

**cell 5** This block writes the results of the query to a Lakehouse table named Analysis.

After running the above blocks, navigate to Lakehouse1 and you can preview the **analysis** table and the *test.csv* output file created.  Note: if the file and table do not appear, you may have to hit the 3 dot ellipsis to the right of Tables and/or the **ExportFromNeo4j** and select 'Refresh' for the UI to update and display that the file and table are there.

