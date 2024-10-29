# Framework

For code visit [github](https://github.com/mentorship-guidance/fire_group_energy_efficiency_etl).

Mentors : 

Mentees :

## Project Structure

## 1. `Configs/`
This directory contains JSON configuration files, which define various parameters used throughout the ETL process.
**`my-project/docs/configs/hourly_generation_by_energy_source.md`**

## 2. `Fetch/`

This package contains modules responsible for data fetching from API.


- **`provider/`**: 
    - **`__init__.py`**: Initializes the provider package.
    - **`EIA/`**: Contains classes and methods for fetching data from the Energy Information Administration (EIA).
        - **`__init__.py`**: Initializes the EIA package.
        - **`eia_data_fetcher.py`**: Implements the `EIADataFetcher` class to retrieve data from the EIA API.
- **`__init__.py`**: Initializes the provider package.
- **`provider_class_manager.py`**: This module manages the different data providers available for fetching data. It initializes the `ProviderClassManager`, which routes requests to the appropriate provider class based on the configuration.


## 3. `Ingest/`

This package contains modules responsible for data ingesting into target database

- **`target/`**: Contains modules for data ingestion into target databases.
    - **`__init__.py`**: Initializes the target module.
    - **`Xata/`**: Contains classes for ingesting data into the Xata database.
        - **`__init__.py`**: Initializes the Xata module.
        - **`xata_data_ingest.py`**: Implements the `XataIngest` class to insert data into the Xata database.
- **`__init__.py`**: Initializes the ingest package.
- **`ingest_class_manager.py`**:  It initializes the `IngestClassManager` ,manages the ingestion process, determining which ingester class to use based on configuration parameters.



### 3. `main.py`

This is the entry point for the ETL process. It orchestrates the entire data pipeline, executing the following steps:

1. Load configuration parameters from a JSON file.
2. Fetch provider parameters for data retrieval.
3. Fetch data from the API using the specified parameters.
4. Preprocess the fetched data (symbol and null value replacements).
5. Ingest transformed data into a specified target database.

### 4. `utils.py`

This module contains utility classes for loading configuration parameters and rules.

- **`ConfigParamsLoader`**: Loads parameters from the JSON configuration file, including provider and ingestion target details.
- **`LoadRule`**: Loads transformation rules, such as symbol and null value replacements, from the configuration.
- **`DataPreprocessor`**: Preprocesses the DataFrame by replacing unwanted symbols and null values according to the loaded rules.

