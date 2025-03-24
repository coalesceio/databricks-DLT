# **Delta Live Tables**

## **Introduction**

The Coalesce DLT node type allows you to create a streaming table,a type
of Delta Live Table.

[Delta Live Tables](https://docs.databricks.com/en/delta-live-tables/index.html)
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

1.Loading file from external location is supported.Loading from
databricks managed volume or external volume is currently not supported.

2.Databricks by default creates tables with lowercase.Hence,it is better to keep table names in lowercase.[https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html](https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html)

3.The node which loads from a file creates a streaming table.For further processing,Re-Sync the columns in the mapping grid using Re-Sync columns button.
The streaming table can be re-created with the Columns inferred using Include Columns Inferred option.

4.The streaming tables can be recreated and refreshed if there is a need to drop the inferred columns or add transformations to columns inferred in previous step.The structure of the streaming table is refreshed only on enabling the 'Refresh Stream' Option

5.Materialized View is currently not supported by DLT node type.

### **DLT Node Configuration**

The DLT has two configuration groups:

* [DLT Node Properties](#dlt-node-properties)
* [DLT Source Data](#dlt-source-data)

<h4 id="dlt-node-properties"> DLT Node Properties </h4>

There are four configs within the **Node Properties** group.

| **Property** | **Description** |
|-------------|-----------------|
| **Storage Location** | Storage Location where the Materialized View will be created |
| **Node Type** | Name of template used to create node objects |
| **Description** | A description of the node's purpose |
| **Deploy Enabled** | If TRUE the node will be deployed / redeployed when changes are detected<br/>If FALSE the node will not be deployed or will be dropped during redeployment |


<h4 id="dlt-source-data"> DLT Source Data </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **Type of Delta Live Table**     | - Streaming Table<br>- Materialized View |
| **Read Files**                   | **True / False** toggle<br/>- **True**: Allows loading from an external cloud location into a streaming table.<br/>- **False**: Allows loading from a table source. |
| **Include Columns Inferred**     | Enables recreating the streaming table with added transformations after syncing columns. |
| **File Location** | External cloud location of the file. |
| **File Format** | - CSV<br/>- JSON |
| **Delimiter** | The delimiter used in case of a CSV file. |
| **Header** | Specifies if a header is needed for CSV file parsing. |
| **Table Properties**             | Defines table properties like quality. |
| **Refresh Stream**               | Enables full refresh of the table if any changes in structure occur. |
| **Partition By** | Specifies the columns used for partitioning the streaming table. |
| **Table Constraints** | - **Primary Key**<br/>- **Foreign Key** |
| **Other Constraints** | - **Column Name**<br/>- **Expectation Expression**<br>- **On Violation Action** |

## **DLT Deployment**

### **DLT Initial Deployment**

When deployed for the first time into an environment the DLT node will
execute the following stage:

-   Create Streaming Table: This stage will execute a CREATE OR REFRESH
    > statement and creates a Streaming Table in the target environment.

### **DLT Redeployment**

If the initial deployment failed,then the redeployment after correcting
any errors successfully deploys the node.

Other scenarios like change in data type,drop or add columns are not
supported

### **DLT Undeployment**

If a DLT Node is deleted from a Workspace, that Workspace is committed
to Git and that commit deployed to a higher level environment then the
Table in the target environment will be dropped.

This is executed in two stages:

-   Delete Table: Coalesce Internal table is dropped.

-   Delete Table: Target table in Snowflake is dropped.
