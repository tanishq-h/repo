## Utilities Documentation

### Overview
The `utils.py` module contains utility classes that assist in loading configuration parameters and handling data preprocessing rules. This module plays a crucial role in ensuring that the ETL pipeline can read and manipulate configuration files effectively.

### Classes

#### 1. ConfigParamsLoader

The `ConfigParamsLoader` class is responsible for loading and retrieving configuration parameters from a specified JSON file. This includes details about data providers, ingestion targets, and rules for data processing.

##### Initialization
```python
def __init__(self, config_path: str):
```
- **Parameters:**
  - `config_path` (str): The path to the JSON configuration file.
  
- **Description:**
  Initializes the `ConfigParamsLoader` instance with the specified configuration path, allowing subsequent methods to access the configuration data.

##### Method: _load_config()
```python
def _load_config(self):
```
- **Returns:**
  - `dict`: The contents of the JSON configuration file.

- **Description:**
  Loads the JSON configuration file and returns it as a Python dictionary. This internal method is used by other methods to access configuration values.

##### Method: get_provider_name()
```python
def get_provider_name(self):
```
- **Returns:**
  - `str`: The provider name specified in the configuration.

- **Description:**
  Retrieves the provider name from the configuration, allowing the ETL pipeline to know which data source to use for fetching data.

##### Method: get_target()
```python
def get_target(self):
```
- **Returns:**
  - `str`: The target specified in the config for data ingestion.

- **Description:**
  Fetches the target for data ingestion, which helps in directing the processed data to the appropriate database or storage solution.

---

#### 2. LoadRule

The `LoadRule` class is responsible for loading and managing rules related to data preprocessing. This includes symbol replacements and null value replacements as defined in the configuration.

##### Initialization
```python
def __init__(self, config_path: str):
```
- **Parameters:**
  - `config_path` (str): The path to the JSON configuration file.
  
- **Description:**
  Initializes the `LoadRule` instance with the configuration path, enabling the loading of preprocessing rules from the configuration.

##### Method: _load_config()
```python
def _load_config(self):
```
- **Returns:**
  - `dict`: The contents of the JSON configuration file.

- **Description:**
  Loads the JSON configuration file similarly to `ConfigParamsLoader`, used to access the preprocessing rules.

##### Method: load_symbol_replacement()
```python
def load_symbol_replacement(self):
```
- **Returns:**
  - `dict`: The symbol replacement rules specified in the configuration.

- **Description:**
  Retrieves the rules for symbol replacement from the configuration. This includes the symbols to be replaced and their corresponding replacements.

##### Method: load_null_value_replacement()
```python
def load_null_value_replacement(self):
```
- **Returns:**
  - `dict`: The null value replacement rules specified in the configuration.

- **Description:**
  Fetches the null value replacement rules from the configuration, specifying what value should be used to replace nulls in the data.

---

#### 3. DataPreprocessor

The `DataPreprocessor` class is responsible for applying preprocessing rules to the fetched data. This includes replacing specified symbols in column headers and filling null values in the DataFrame.

##### Initialization
```python
def __init__(self, symbol_replacement, null_value_replacement):
```
- **Parameters:**
  - `symbol_replacement` (dict): A dictionary containing symbol replacement rules.
  - `null_value_replacement` (dict): A dictionary containing null value replacement rules.
  
- **Description:**
  Initializes the `DataPreprocessor` with the specified rules for symbol and null value replacements, setting up the instance for data cleaning operations.

##### Method: replace_symbols()
```python
def replace_symbols(self, df):
```
- **Parameters:**
  - `df` (pd.DataFrame): The DataFrame whose column headers will be modified.

- **Returns:**
  - `pd.DataFrame`: The DataFrame with modified column headers.

- **Description:**
  Iterates over the defined symbol replacement rules and applies them to the column headers of the provided DataFrame. Symbols specified in the rules are replaced with their designated replacements.

##### Method: replace_null()
```python
def replace_null(self, df):
```
- **Parameters:**
  - `df` (pd.DataFrame): The DataFrame in which null values will be replaced.

- **Returns:**
  - `pd.DataFrame`: The DataFrame with null values replaced.

- **Description:**
  Fills null values in the DataFrame with the specified replacement value. This ensures that the DataFrame does not contain any null entries before ingestion.

---

### Conclusion
The `utils.py` module provides essential utility functions that facilitate the smooth operation of the ETL pipeline. The `ConfigParamsLoader` allows for the easy loading of configuration parameters, while the `LoadRule` and `DataPreprocessor` classes enable effective data cleaning and preparation for ingestion. This modular approach ensures that the ETL process remains flexible and maintainable.