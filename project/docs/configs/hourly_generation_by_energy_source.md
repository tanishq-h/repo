# Hourly Generation by Energy Source Configuration

## Overview
This configuration file is used to fetch hourly generation data by energy source from the EIA API. It contains details about the data provider, API endpoint, parameters for the API request, and rules for data transformation before ingestion into the Xata database.

## Configuration Structure

#### **Providers**
The `providers` section contains all the necessary information to access the provider API.


- **provider**: 
  - **Type**: `string`
  - **Description**: Specifies the data provider. In our case, it is set to "EIA".

- **base_url**: 
  - **Type**: `string`
  - **Description**: The base URL for the EIA API endpoint from which the data will be fetched.
  - **Example**: `https://api.eia.gov/v2/electricity/rto/fuel-type-data/data/`

- **params**: 
  - **Type**: `object`
  - **Description**: Contains various parameters for the API request.

#### **Params Details**

  - **frequency**: 
    - **Type**: `string`
    - **Description**: The frequency of the data. Here, it is set to "hourly".

  - **data**: 
    - **Type**: `array`
    - **Description**: Specifies which data fields to retrieve. 
    - **Example**: `["value"]` indicates that only the "value" field will be fetched.

  - **facets**: 
    - **Type**: `object`
    - **Description**: Contains filtering options for the data.

    - **fueltype**: 
      - **Type**: `array`
      - **Description**: Specifies the types of fuel to include in the data fetch. 
      - **Example**: `["COL", "NG", "NUC", "OIL", "OTH", "SUN", "UNK", "WAT", "WND"]`.

    - **respondent**: 
      - **Type**: `array`
      - **Description**: Specifies the respondents from whom the data is being fetched. 
      - **Example**: `["AEC", "AECI", "AVA", "AVRN", "AZPS", ...]`.

  - **start**: 
    - **Type**: `string` or `null`
    - **Description**: The start date for the data fetch. Setting this to `null` indicates that it will fetch all available data from the beginning.

  - **end**: 
    - **Type**: `string` or `null`
    - **Description**: The end date for the data fetch. Setting this to `null` indicates that it will fetch data up to the current date.

  - **sort**: 
    - **Type**: `array`
    - **Description**: Specifies the sorting order of the fetched data.
    - **Example**: 
      ```json
      [
          {"column": "fueltype", "direction": "desc"},
          {"column": "respondent", "direction": "asc"},
          {"column": "period", "direction": "asc"},
          {"column": "value", "direction": "asc"}
      ]
      ```
      This indicates sorting first by `fueltype` in descending order, followed by `respondent`, `period`, and `value` in ascending order.

  - **offset**: 
    - **Type**: `integer`
    - **Description**: The offset for pagination. It determines where to start fetching records.
    - **Example**: `0` means start from the beginning.

  - **length**: 
    - **Type**: `integer`
    - **Description**: The maximum number of records to fetch in a single request.
    - **Example**: `5000` indicates that up to 5000 records will be fetched.

### Ingest
The `ingest` section defines where the fetched data will be stored.

- **target**: 
  - **Type**: `string`
  - **Description**: Specifies the target database for ingestion. 
  - **Example**: `"Xata"` indicates that the data will be ingested into the Xata database.

- **table_name**: 
  - **Type**: `string`
  - **Description**: The name of the table in the target database where the data will be stored.
  - **Example**: `"test"` indicates the name of the table.

### Rules
The `rules` section contains transformation rules for the data before ingestion.

- **symbol_replacement**: 
  - **Type**: `object`
  - **Description**: Rules for replacing specific symbols in the data.

  - **replacements**: 
    - **Type**: `array`
    - **Description**: Specifies the symbols to replace and their replacements.
    - **Example**:
      ```json
      [
          {
              "symbols": ["-"],
              "replace_with": "_"
          }
      ]
      ```
      This indicates that all hyphens (`-`) in the data will be replaced with underscores (`_`).

- **null_value_replacement**: 
  - **Type**: `object`
  - **Description**: Specifies how to handle null values in the data.
  
  - **replace_with**: 
    - **Type**: `integer`
    - **Description**: The value to replace nulls with.
    - **Example**: `0` indicates that null values will be replaced with `0`.

## Conclusion
This configuration file is essential for retrieving, transforming, and ingesting hourly generation data by energy source from the EIA API into the Xata database.
```
