# **Delta Live Tables**

## **Introduction**

The Coalesce DLT node type allows you to create a streaming table,a type
of Delta Live Table.

[Delta Live Tables](https://docs.databricks.com/aws/en/dlt/streaming-tables)
is a declarative framework for building reliable, maintainable, and
testable data processing pipelines. Delta Live Tables manages how your
data is transformed based on queries you define for each processing
step. You can also enforce data quality with Delta Live Tables
expectations, which allow you to define expected data quality and
specify how to handle records that fail those expectations.

Databricks recommends using streaming tables to ingest data using
Databricks SQL. A [streaming
table](https://docs.databricks.com/en/tables/streaming.html#additional-resources)
is a table registered to Unity Catalog with extra support for streaming
or incremental data processing. A Delta Live Tables pipeline is
automatically created for each streaming table.

## **Key points**

1.Loading file from external location/databricks managed volume/external volume is currently supported.

2.Databricks by default creates tables with lowercase.Hence,it is better to keep table names in lowercase.[https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html](https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html)

3.The node which loads from a file creates a streaming table.For further processing,Re-Sync the columns in the mapping grid using Re-Sync columns button.
The streaming table can be re-created with the Columns inferred using Include Columns Inferred option.

![dlt-resync](https://github.com/user-attachments/assets/8199536e-ef4d-4b24-ab69-ce8c89356938)

4.The streaming tables can be recreated and refreshed if there is a need to drop the inferred columns or add transformations to columns inferred in previous step.The structure of the streaming table is refreshed only on enabling the 'Refresh Stream' Option

### **DLT Node Configuration**

The DLT has two configuration groups:

* [DLT Node Properties](#dlt-node-properties)
* [General Options](#general-options)
* [DLT Options](#dlt-options)
* [Schedule Options](#schedule-options)

<h4 id="dlt-node-properties"> DLT Node Properties </h4>

There are four configs within the **Node Properties** group.

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | Storage Location where the Materialized View will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |

### General Options

| **Option** | **Description** |
|------------|----------------|
| **Create As** | Choose materialization type-by default it is STREAMING TABLE  |
|**Schedule refresh**| True / False toggle<br/>- **True**: Schedule Option will be visible<br/>- **False**:  Schedule Option will be disabled|

<h4 id="dlt-options"> DLT options </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **Type of Delta Live Table**     | - Streaming Table <br/>- Materialized View |
| **Read Files**                   | **True / False** toggle <br/>- **True**: Allows loading from an external cloud location into a streaming table.<br/>- **False**: Allows loading from a table source. |
| **Include Columns Inferred**     | Enables recreating the streaming table with added transformations after syncing columns. |
| **File Location** | External cloud location of the file. |
| **File Format** | - CSV <br/>- JSON |
| **Delimiter** | The delimiter used in case of a CSV file. |
| **Header** | Specifies if a header is needed for CSV file parsing. |
| **Table Properties**             | Defines table properties like quality. |
| **Refresh Stream**               | Enables full refresh of the table if any changes in structure occur. |
| **Partition By** | Specifies the columns used for partitioning the streaming table. |
| **Table Constraints** | - **Primary Key**<br/>- **Foreign Key** |
| **Other Constraints** | - **Column Name**<br/>- **Expectation Expression**<br/>- **On Violation Action** |

### Schedule Options

Schedule Options is available only when Schedule Refresh toggle is True

| **Option** | **Description** |
|------------|----------------|
|**Task Schedule**| Options in Task Schedule<br/>- Periodic Schedule<br/>- CRON    |
|**Schedule refesh-time period**|Available when Task Schedule is Set to Periodic Schedule<br/>Options in Schedule refesh-time period<br/>-Every Hours<br/>-Every Days<br/>-Every Weeks |
|**Specific interval of periodic refresh(integer value)**|Available when Task Schedule is Set to Periodic Schedule |
|**CRON string**|Available when Task Schedule is Set to CRON|
|**CRON TIME ZONE**|Available when Task Schedule is Set to CRON |

## **DLT Deployment**

### Initial Deployment
When deployed for the first time into an environment DLT node will execute three stages:

| **Stage** | **Description** |
|-----------|----------------|
| **Create Streaming table** | This stage will execute a CREATE OR REFRESH statement and create a Streaming table in the target environment |

### Redeployment

After the Streaming table has deployed for the first time into a target environment, subsequent deployments may result in either altering the Streaming table or recreating the Streaming table.

If a Streaming table is to be altered this will run the following stage:

#### Altering the Streaming table

The following config changes trigger ALTER statements:

1. Add schedule
2. Alter schedule
3. Drop schedule

These execute the two stages:

| **Stage** | **Description** |
|-----------|----------------|
| **Alter Streaming table** | Executes ALTER to modify parameters |

#### Recreating the Streaming table

If anything changes other than the configuration options specified above then the Streaming table will be recreated by running a CREATE OR REPLACE statement.

### Undeployment

If a Streaming table is deleted from a Workspace, that Workspace is committed to Git and that commit deployed to a higher-level environment then the Streaming table in the target environment will be dropped.

This is executed as a single stage:

| **Stage** | **Description** |
|-----------|----------------|
| **Drop Streaming table** | Removes the Streaming table |

## Code

* [Node definition](https://github.com/coalesceio/databricks-DLT/blob/main/nodeTypes/DLT-368/definition.yml)
* [Create Template](https://github.com/coalesceio/databricks-DLT/blob/main/nodeTypes/DLT-368/create.sql.j2)
* [Run Template](https://github.com/coalesceio/databricks-DLT/blob/main/nodeTypes/DLT-368/run.sql.j2)
