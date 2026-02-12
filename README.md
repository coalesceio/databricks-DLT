# **Lakeflow Declarative Pipelines**

## **Brief Summary**

The Coalesce LDP node type allows you to create a streaming table.
A streaming table is a form of Unity Catalog managed table that is also a streaming target for Lakeflow Declarative Pipelines.

[Lakeflow Declarative Pipelines](https://docs.databricks.com/aws/en/dlt/concepts)
is a declarative framework for developing and running batch and streaming data pipelines in SQL and Python. Lakeflow Declarative Pipelines runs on the performance-optimized Databricks Runtime (DBR), and the Lakeflow Declarative Pipelines flows API uses the same DataFrame API as Apache Spark and Structured Streaming. Common use cases for Lakeflow Declarative Pipelines include incremental data ingestion from sources such as cloud storage (including Amazon S3, Azure ADLS Gen2, and Google Cloud Storage) and message buses (such as Apache Kafka, Amazon Kinesis, Google Pub/Sub, Azure EventHub, and Apache Pulsar), incremental batch and streaming transformations with stateless and stateful operators, and real-time stream processing between transactional stores like message buses and databases.

A [streaming table](https://docs.databricks.com/aws/en/dlt/streaming-tables) is a Delta table with additional support for streaming or incremental data processing. A streaming table can be targeted by one or more flows in an ETL pipeline.

## **Key points**

1.Databricks by default creates tables with lowercase.Hence,it is better to keep table names in lowercase.[https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html](https://docs.databricks.com/en/sql/language-manual/sql-ref-names.html)

2.The node which loads from a file creates a streaming table.For further processing,Re-Sync the columns in the mapping grid using Re-Sync columns button.
The streaming table can be re-created with the Columns inferred using Include Columns Inferred option.

![dlt-resync](https://github.com/user-attachments/assets/8199536e-ef4d-4b24-ab69-ce8c89356938)

3.The streaming tables can be recreated and refreshed if there is a need to drop the inferred columns or add transformations to columns inferred in previous step.The structure of the streaming table is refreshed only on enabling the 'Refresh Stream' Option

### **LDP Node Configuration**

The LDP has two configuration groups:

* [LDP Node Properties](#ldp-node-properties)
* [General Options](#general-options)
* [LDP Options](#ldp-options)
* [File Options](#file-options)
* [General file load Options](#general-file-load-options)
* [File format options](#file-format-options)
* [Advanced file load options](#advanced-file-load-options)
* [Schedule Options](#schedule-options)

<h4 id="dlt-node-properties"> LDP Node Properties </h4>

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

<h4 id="ldp-options"> LDP options </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **Type of Lakeflow Declarative Pipeline**     | - Streaming Table |
| **Read Files**                   | **True / False** toggle <br/>- **True**: Allows loading from an external cloud location into a streaming table.<br/>- **False**: Allows loading from a table source. |
| **Include Columns Inferred**     | Enables recreating the streaming table with added transformations after syncing columns. |
| **Table Properties**             | Defines table properties like quality. |
| **Refresh Stream**               | Enables full refresh of the table if any changes in structure occur. |
| **Partition By** | Specifies the columns used for partitioning the streaming table. |
| **Table Constraints** | - **Primary Key**<br/>- **Foreign Key** |
| **Other Constraints** | - **Column Name**<br/>- **Expectation Expression**<br/>- **On Violation Action** |


<h4 id="file-options"> File options </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **File Location** | External cloud location of the file. |
| **File Name/File Pattern**     | The pattern to identify files to be processed. |
| **File Type**                  | - Options: **CSV**, **JSON**, **Parquet**, **Avro**, **ORC**, **Text**, **XML**. |
| **Advanced File Load Options** | Toggle to enable additional loading options for files. |
| **Advanced File Format Options** | Toggle to enable additional formatting options for files. |

<h4 id="general-file-load-options"> General File Load options </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **Schema Definition**          | The schema to define the structure of the input data. |
| **Override inferred schema columns**| The schema to define the structure of the input data. |
| **Infer Exact Column Types**   | Toggles whether to infer column data types accurately. |
| **Ignore Corrupt Files**       | Toggle to ignore files that are corrupt during processing. |
| **Ignore Missing Files**       | Toggle to ignore files that are missing during processing. |
| **Partition Columns**          | Comma-separated list of Hive-style partition columns to include|

<h4 id="advanced-file-load-options"> Advanced File Load options </h4>

| **Option**                     | **Description** |
|----------------------------------|---------------|
| **Process files modified after (Ex '2024-01-01T00:00:00Z')**   | Only process files modified after this timestamp |
| **Process files modified before (Ex'2024-12-31T23:59:59Z')**       | Only process files modified before this timestamp |
| **Recursive file lookup**       | Search nested directories regardless of naming scheme. Default: false |
| **Use strict glob pattern matching**          | Use strict glob pattern matching. Default: true|

<h4 id="file-format-options"> File Format options </h4>

<h5 id="parquet-options">PARQUET Options</h5>

| **Group**        | **Option**            |  **Description**                                            |
|------------------|-----------------------|------------------------------------------------------------|
| Options          | Merge Schema           |  Infer and merge schema across multiple files. Default: false |
| Options          | Column name for rescued data column |  Column name to collect data that doesn't match schema     |
| Advanced         | Datetime rebase mode    | Rebasing DATE/TIMESTAMP between Julian and Proleptic Gregorian calendars. Options: EXCEPTION, LEGACY, CORRECTED. Default: LEGACY |
| Advanced         | Integer rebase mode       | Rebasing INT96 timestamps. Options: EXCEPTION, LEGACY, CORRECTED. Default: LEGACY |

<h5 id="avro-options">AVRO Options</h5>

| **Group**        | **Option**            | **Description**                                            |
|------------------|-----------------------|---------------------------------------------------------------|
| Options          | avroSchema            | User-provided Avro schema (can be evolved schema)        |
| Options          | Merge Schema          | Infer and merge schema across files. Default: false       |
| Options          | Column name for rescued data column     |Column name to collect data that doesn't match schema     |
| Advanced         | Datetime rebase mode    | Rebasing DATE/TIMESTAMP between Julian and Proleptic Gregorian calendars. Options: EXCEPTION, LEGACY, CORRECTED. Default: LEGACY |
| Advanced         | Integer rebase mode       | Rebasing INT96 timestamps. Options: EXCEPTION, LEGACY, CORRECTED. Default: LEGACY |

<h5 id="orc-options">ORC Options</h5>

| **Group**        | **Option**            |  **Description**                                            |
|------------------|-----------------------|------------------------------------------------------------|
| Options          | Merge Schema           | Infer and merge schema across files. Default: false       |


<h5 id="xml-options">XML Options</h5>

| **Group**           | **Option**            |  **Description**                                            |
|---------------------|-----------------------|------------------------------------------------------------|
| Options             | Row Tag                | Required. XML element to treat as a row (e.g., 'book' for `<books><book>...</book></books>`) |
| Options             | Encoding               |Character encoding. Default: UTF-8                        |
| Options             | String representation of null | String representation of null. Default: null             |
| Options             | Column name for rescued data column| Column name to collect data that doesn't match schema     |
| Advanced            | Prefix for attribute  |Prefix for attribute field names. Default: _              |
| Advanced            | valueTag              | Tag for character data in elements with attributes. Default: _VALUE |
| Advanced            | Exclude attributes from elements | Exclude attributes from elements. Default: false          |
| Advanced            | Skip surrounding whitespace| Skip surrounding whitespace. Default: true                |
| Advanced            | Ignore namespace | Ignore namespace prefixes. Default: false                 |
| Advanced            | Sampling Ratio        | Fraction of rows for schema inference. Default: 1.0       |
| Advanced            | Mode                  | Corrupt record handling. Options: PERMISSIVE, DROPMALFORMED, FAILFAST. Default: PERMISSIVE |
| Advanced            | Date Parsing Format   | Date parsing format. Default: yyyy-MM-dd                  |
| Advanced            | Timestamp Parsing Format | Timestamp parsing format. Default: yyyy-MM-dd'T'HH:mm:ss[.SSS][XXX] |
| Advanced            | Path to XSD for row validation| Path to XSD for row validation       

<h5 id="text-options">TEXT Options</h5>

| **Group**        | **Option**            |  **Description**                                            |
|------------------|-----------------------|------------------------------------------------------------|
| Options          | encoding              | Character encoding. Default: UTF-8                        |
| Options          | Line Separator               | Line separator string. Default: auto-detects \r, \r\n, \n |
| Advanced         | Whole text            | Read entire file as single record. Default: false         |


<h5 id="json-options">JSON Options</h5>

| **Group**          | **Option**            | **Description**                                            |
|--------------------|-----------------------|------------------------------------------------------------
| Options            | Encoding              |  Character encoding. Default: UTF-8                        |
| Options            | quote                 | Quote character. Default: "                               |
| Options            | escape                | Escape character. Default: \                               |
| Advanced           | Skip surrounding whitespace| Skip surrounding whitespace. Default: true                |
| Options            | Number of rows to skip from beginning      | Number of rows to skip from beginning. Default: 0         |
| Options            | Column name for rescued data column  | Column name for rescued data                              |
| Advanced           | MultiLine             | Records span multiple lines. Default: false               |
| Advanced           | Mode                  | Options: PERMISSIVE, DROPMALFORMED, FAILFAST. Default: PERMISSIVE |
| Advanced           | Character indicating line comment| Character indicating line comment. Default: disabled       |
| Advanced           | dateFormat            | Date parsing format. Default: yyyy-MM-dd                  |
| Advanced           | timestampFormat       | Timestamp parsing format                                   |
| Advanced           | Ignore Leading WhiteSpaces| Default: false                                            |
| Advanced           | ignore Trailing WhiteSpaces| toggleButton     | Default: false                                            |
| Advanced           | Allowcomments | Allow comments in JSON data. Default: false       |
| Advanced           | AllowUnquotedFieldNames|Allow unquoted field names. Default: false        |
| Advanced           | InferPrimitiveTypes    | Infer primitive types correctly. Default: true     |

<h5 id="csv-options">CSV Options</h5>

| **Group**          | **Option**            | **Description**                                            |
|--------------------|-----------------------|------------------------------------------------------------|
| Options            | Delimiter             |  Delimiter used in csv files                              |
| Options            | Header added          |  Header added in csv file                                 |
| Options            | Encoding              |  Character encoding. Default: UTF-8                        |
| Options            | quote                 | Quote character. Default: "                               |
| Options            | escape                | Escape character. Default: \                               |
| Advanced           | Skip surrounding whitespace| Skip surrounding whitespace. Default: true                |
| Options            | Number of rows to skip from beginning      | Number of rows to skip from beginning. Default: 0         |
| Options            | Column name for rescued data column  | Column name for rescued data                              |
| Advanced           | MultiLine             | Records span multiple lines. Default: false               |
| Advanced           | Mode                  | Options: PERMISSIVE, DROPMALFORMED, FAILFAST. Default: PERMISSIVE |
| Advanced           | Character indicating line comment| Character indicating line comment. Default: disabled       |
| Advanced           | dateFormat            | Date parsing format. Default: yyyy-MM-dd                  |
| Advanced           | timestampFormat       | Timestamp parsing format                                   |
| Advanced           | Ignore Leading WhiteSpaces| Default: false                                            |
| Advanced           | ignore Trailing WhiteSpaces| toggleButton     | Default: false                                            |

### Schedule Options

Schedule Options is available only when Schedule Refresh toggle is True

| **Option** | **Description** |
|------------|----------------|
|**Task Schedule**| Options in Task Schedule<br/>- Periodic Schedule<br/>- CRON    |
|**Schedule refesh-time period**|Available when Task Schedule is Set to Periodic Schedule<br/>Options in Schedule refesh-time period<br/>-Every Hours<br/>-Every Days<br/>-Every Weeks |
|**Specific interval of periodic refresh(integer value)**|Available when Task Schedule is Set to Periodic Schedule |
|**CRON string**|Available when Task Schedule is Set to CRON|
|**CRON TIME ZONE**|Available when Task Schedule is Set to CRON |

## **LDP Deployment**

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
