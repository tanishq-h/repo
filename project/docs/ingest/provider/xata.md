# Xata Ingest and Ingest Class Manager Documentation

## Overview

This documentation provides an overview of the `XataIngest` class and the `IngestClassManager` class, which are used for data ingestion into a XATA database using configuration files and pandas DataFrames. The `IngestClassManager` acts as a manager for different ingesting classes.

## Classes

### 1. `IngestClassManager`

The `IngestClassManager` is responsible for managing the ingestion process. It allows for easy extension to include multiple ingesting classes.

#### Initialization

`def __init__(self, config_path: str):`
Initializes  the IngestClassManager with a specified configuration path and a mapping of available ingesting classes.

#### Mapping

`self.ingesters = {'Xata': XataIngest}`:
Maps the target for ingestion with the corresponding ingester class.


#### Method: ingest_data()

`def ingest_data(self, df: pd.DataFrame, target: str):`This method ingests data using the specified target.

##### Arguments:

`df`: The DataFrame containing data to be ingested.

`target`: The target for data ingestion (e.g., "Xata").

##### Description:

Looks up the appropriate ingester class based on the provided target name.
If the target is not found, it logs an error and raises an exception.
Creates an instance of the selected ingester class, passing the configuration path and DataFrame.
Calls the ingest_data method of the ingester instance to perform the actual data ingestion.


### 2. `XataIngest`

The `XataIngest` class is designed to handle data ingestion into a XATA database.

#### Initialization

`def __init__(self, config_path: str, df: pd.DataFrame):`
Initializes  the XataIngest with a specified configuration path and DataFrame containing data to be ingested.

#### Method: ingest_data()
`def ingest_data(self):`
Ingests data into the specified database table.

##### Description

- Establishes a connection to the XATA database using the DSN.
- Converts the DataFrame into a list of records.
- Constructs an SQL INSERT statement for the target table.
- Executes the insertion within a transaction to ensure data integrity.
- Commits the transaction if successful; rolls back if an error occurs.
- Closes the database connection and logs the outcome.

##### Logging:

- Logs the number of records inserted on success.
- Logs errors that occur during the insertion process.