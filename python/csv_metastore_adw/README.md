# Overview

This example shows you how to use OCI Data Flow to process data in OCI Object Store and save the results to Oracle ADW or ATP or metastore.
###_Note_: To test this scample locally use Data flow code editor plugin
## Prerequisites

Before you begin:

1. Ensure your tenant is configured for Data Flow by following [instructions](https://docs.cloud.oracle.com/en-us/iaas/data-flow/using/dfs_getting_started.htm#set_up_admin)
2. Provision an ADW or ATP instance.
3. Create a wallet for your ADW/ATP instance.
4. (Optional, strongly recommended): Use Dataflow code editor to test your code locally before deploying to Data Flow.

## Load Required Data

Upload a sample CSV file to OCI object store.

## Application Setup

Customize ```csvToMetastoreToADW.py``` with:

* Set INPUT_PATH to the OCI path of your CSV data.
* Set PASSWORD of ADW instance.
* Set databaseName db in ADW where data is to be written.
* Set tableName table name argument in ADW where data is to be written. 
* Set CONNECTION_ID to a TNS name valid for the database.
* Set USER to the user who generated the wallet file.
* Set WALLET_PATH to the path on object store for the wallet.

  Test the Application Locally (recommended):
  You can test the application in code editor data flow plugin locally using Run locally:

## Deploy and Run the Application

* Copy csvToMetastoreToADW.py to object store or upload csvToMetastoreToADW.py from Dataflow upload artifact utility.
* Create a Data Flow Python application. Be sure to include archive.zip(if required) as the dependency archive.
  * Refer [here](https://docs.cloud.oracle.com/en-us/iaas/data-flow/using/dfs_data_flow_library.htm#create_pyspark_app) for more information.
* Run the application.

## Run the Application using OCI Cloud Shell or OCI CLI


Create a bucket. Alternatively you can re-use an existing bucket.

```sh
oci data-flow application create \
    --compartment-id <compartment_ocid> \
    --display-name "PySpark Metastore to ADW" \
    --driver-shape VM.Standard2.1 \
    --executor-shape VM.Standard2.1 \
    --num-executors 1 \
    --spark-version 2.4.4 \
    --file-uri oci://<bucket>@<namespace>/csvToMetastoreToADW.py \
    --arguments --table <table_name>
    --language Python
oci data-flow run create \
    --application-id <application_ocid> \
    --compartment-id <compartment_ocid> \
    --application-id <application_ocid> \
    --display-name 'PySpark Metastore to ADW"
```
