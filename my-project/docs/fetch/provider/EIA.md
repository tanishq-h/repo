# EIA Data Fetcher and Provider Class Manager Documentation

## Overview

This documentation provides an overview of the `ProviderClassManager` and `EIADataFetcher` classes, which are responsible for fetching energy data from the EIA API. The `ProviderClassManager` manages different data provider classes and orchestrates the data retrieval process..

## Classes

### 1. `ProviderClassManager`

The `ProviderClassManager` is responsible for managing the data fetching process from various providers.

#### Initialization

`def __init__(self, config_path: str):`
Initializes the ProviderClassManager with a specified configuration path and a mapping of available data provider classes.

#### Mapping

`self.providers = {'EIA': EIADataFetcher}:`
Maps the provider name to the corresponding data fetching class.


#### Method: fetch_data()

`def fetch_data(self, provider_name: str):`This method fetches data using the specified provider.


##### Arguments:

`provider_name:` The name of the provider to fetch data from (e.g., "EIA").

##### Description:

Looks up the appropriate provider class based on the provided provider name. If the provider is not found, it logs an error and raises a ValueError.
Creates an instance of the selected provider class, passing the configuration path.
Calls the fetch_data method of the provider instance to perform the actual data fetching.


### 2. `EIADataFetcher`

The `EIADataFetcher` class handles the interaction with the EIA API, managing API requests and data retrieval.

#### Initialization

`def __init__(self, config_path: str):`
Initializes the EIADataFetcher with a specified configuration path.

#### Method: fetch_data()
`def fetch_data(self) -> pd.DataFrame:`
Fetches data from the EIA API, handling pagination as necessary.


##### Description

- Establishes a connection to the EIA API and constructs query parameters.
- Sends GET requests to the API and processes the responses.
- Normalizes the JSON response data into a Pandas DataFrame.
- Executes the insertion within a transaction to ensure data integrity.
- Handles pagination by iterating through the data until all available records are fetched.
- Raises exceptions if the response status is not successful or the data format is unexpected.

##### Logging:

- Logs the number of records fetched with each API call.
- Logs errors that occur during the data fetching process, ensuring traceability.