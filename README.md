Here’s a basic Python script that demonstrates how to fetch data from Teradata SQL and load it into Google BigQuery, with proper handling of data types.

### Requirements:
1. **Python libraries**:
   - `teradatasql` to connect to Teradata.
   - `pandas` to handle data manipulation.
   - `google-cloud-bigquery` to upload the data into BigQuery.
   
2. **Authentication**: 
   Make sure your Google Cloud SDK is set up and you've authenticated using a service account that has permissions to load data into BigQuery.

You can install the necessary packages using:
```bash
pip install teradatasql pandas google-cloud-bigquery
```

### Script:
```python
import teradatasql
import pandas as pd
from google.cloud import bigquery

# Teradata connection details
TERADATA_HOST = 'your_teradata_host'
TERADATA_USER = 'your_teradata_username'
TERADATA_PASSWORD = 'your_teradata_password'
TERADATA_QUERY = 'SELECT * FROM your_table'

# BigQuery settings
BIGQUERY_PROJECT = 'your_gcp_project_id'
BIGQUERY_DATASET = 'your_dataset'
BIGQUERY_TABLE = 'your_table'

# Fetch data from Teradata
def fetch_data_from_teradata(query):
    with teradatasql.connect(f'{"hostname=" + TERADATA_HOST}', user=TERADATA_USER, password=TERADATA_PASSWORD) as connection:
        df = pd.read_sql(query, connection)
        return df

# Load DataFrame to BigQuery
def load_data_to_bigquery(df, project, dataset, table):
    client = bigquery.Client(project=project)
    table_id = f'{project}.{dataset}.{table}'
    
    # Define the schema based on the DataFrame
    job_config = bigquery.LoadJobConfig()
    
    # Convert Pandas DataFrame dtypes to BigQuery dtypes
    job_config.schema = [
        bigquery.SchemaField(column, get_bq_type(df[column].dtype)) 
        for column in df.columns
    ]
    
    # Load data into BigQuery
    job = client.load_table_from_dataframe(df, table_id, job_config=job_config)
    
    # Wait for the load job to complete
    job.result()
    print(f"Loaded {job.output_rows} rows into {table_id}.")

# Function to map Pandas dtypes to BigQuery types
def get_bq_type(dtype):
    if pd.api.types.is_integer_dtype(dtype):
        return 'INTEGER'
    elif pd.api.types.is_float_dtype(dtype):
        return 'FLOAT'
    elif pd.api.types.is_bool_dtype(dtype):
        return 'BOOLEAN'
    elif pd.api.types.is_datetime64_any_dtype(dtype):
        return 'TIMESTAMP'
    else:
        return 'STRING'

if __name__ == "__main__":
    # Fetch data from Teradata SQL
    df = fetch_data_from_teradata(TERADATA_QUERY)
    
    # Load data into Google BigQuery
    load_data_to_bigquery(df, BIGQUERY_PROJECT, BIGQUERY_DATASET, BIGQUERY_TABLE)
```

### Explanation:
1. **Fetching Data from Teradata**:
   - The script connects to Teradata using the `teradatasql` connector and retrieves the result of the SQL query into a Pandas DataFrame.

2. **Handling Data Types**:
   - The `get_bq_type()` function ensures that the Pandas data types are converted to corresponding BigQuery data types (`INTEGER`, `FLOAT`, `STRING`, etc.).

3. **Uploading to BigQuery**:
   - The data from the DataFrame is loaded into BigQuery using `bigquery.Client()`. The schema is defined based on the DataFrame's data types.

### Notes:
- Make sure to replace placeholders (`your_teradata_host`, `your_gcp_project_id`, etc.) with actual values.
- This script assumes basic data types. For complex data types (arrays, structs), you may need to adjust the schema and data conversion logic.

Let me know if you need further adjustments or clarifications!