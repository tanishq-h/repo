# Main Logic Documentation

## Overview
The main.py module orchestrates the entire data processing pipeline for the Energy Efficiency ETL project. It is responsible for loading configuration parameters, fetching data from specified providers, preprocessing the data, and ultimately ingesting it into the target database.

### Method: run_pipeline(config_path)
This is the main function that coordinates the execution of the ETL process.


Here’s a detailed explanation of the steps involved in the `main.py` logic, along with a flow chart that illustrates the function invocation process for the Energy Efficiency ETL pipeline.


#### Steps Involved

1. **Load Configuration Parameters**
   - The `ConfigParamsLoader` class is instantiated with the provided `config_path`.
   - Configuration parameters are loaded from the specified JSON file.
   - The provider name is extracted from the configuration for later use.


   config_loader = ConfigParamsLoader(config_path)
   provider_name = config_loader.get_provider_name()

2. **Fetch Provider Parameters**
   - The `ProviderClassManager` class is instantiated with the same configuration path.
   - This manager retrieves the appropriate provider class based on the extracted provider name.
   - The selected provider class is then called to fetch data from the API.

   ```python
   provider_manager = ProviderClassManager(config_path)
   df = provider_manager.fetch_data(provider_name)
   ```

3. **Data Preprocessing**
   - The `LoadRule` class is instantiated to load any preprocessing rules defined in the configuration.
   - The symbols to be replaced and the values to replace nulls with are retrieved.
   - A `DataPreprocessor` instance is created, which applies the specified rules to clean the fetched DataFrame.

   ```python
   rule = LoadRule(config_path)
   symbol_replacement = rule.load_symbol_replacement()
   null_value_replacement = rule.load_null_value_replacement()
   preprocessor = DataPreprocessor(symbol_replacement, null_value_replacement)
   df = preprocessor.replace_symbols(df)
   df = preprocessor.replace_null(df)
   ```

4. **Ingest Process**
   - The preprocessed DataFrame is prepared for ingestion.
   - The `IngestClassManager` is used to manage the ingestion process, determining which ingester to use based on the target specified in the configuration.
   - The data is ingested into the specified database.

   ```python
   target = config_loader.get_target()
   ingest_manager = IngestClassManager(config_path)
   ingest_manager.ingest_data(df, target)
   ```

5. **Completion**
   - The pipeline completes, and a message is printed to indicate that all processes have been executed successfully.

### Flow Chart
Below is a flow chart that illustrates the function invocation process for the ETL pipeline. You can visualize how each component interacts within the main pipeline.




### Conclusion
The `main.py` file serves as the backbone of the ETL process, guiding the flow from configuration loading to data ingestion. Each step is modular, allowing for easy modifications or extensions of the pipeline in the future. The detailed workflow ensures that the data is efficiently and accurately processed from source to target.