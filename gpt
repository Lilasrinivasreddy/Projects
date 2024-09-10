import argparse
import teradatasql
import pandas as pd
from google.cloud import bigquery
import os

# Teradata connection details (these can also be dynamically passed if needed)
TERADATA_HOST = 'your_teradata_host'
TERADATA_USER = 'your_teradata_username'
TERADATA_PASSWORD = 'your_teradata_password'

# BigQuery settings (these can also be dynamically passed if needed)
BIGQUERY_PROJECT = 'your_gcp_project_id'
BIGQUERY_DATASET = 'your_dataset'
BIGQUERY_TABLE = 'your_table'

# Function to fetch data from Teradata table
def fetch_data_from_teradata(query):
    with teradatasql.connect(f'{"hostname=" + TERADATA_HOST}', user=TERADATA_USER, password=TERADATA_PASSWORD) as connection:
        df = pd.read_sql(query, connection)
        return df

# Function to load data from CSV stored in Teradata
def load_data_from_csv_in_teradata(csv_file_path):
    query = f"SELECT * FROM {csv_file_path}"  # Assuming Teradata allows querying CSV files in this manner
    return fetch_data_from_teradata(query)

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

# Main function to handle logic of CSV or Table in Teradata
def main():
    # Setup argument parser for dynamic input
    parser = argparse.ArgumentParser(description="Fetch data from Teradata and load it into BigQuery")
    parser.add_argument('--data_type', type=str, required=True, choices=['csv', 'table'],
                        help="Type of data source: 'csv' for CSV file or 'table' for Teradata table")
    parser.add_argument('--source', type=str, required=True,
                        help="For CSV: Path to the CSV file in Teradata, for Table: Teradata table name")
    parser.add_argument('--project', type=str, required=True,
                        help="Google Cloud project ID")
    parser.add_argument('--dataset', type=str, required=True,
                        help="BigQuery dataset name")
    parser.add_argument('--table', type=str, required=True,
                        help="BigQuery table name")
    
    args = parser.parse_args()
    
    # Fetch data based on the data type
    if args.data_type == "csv":
        print("Loading data from CSV stored in Teradata...")
        df = load_data_from_csv_in_teradata(args.source)
    elif args.data_type == "table":
        print("Fetching data from Teradata table...")
        query = f"SELECT * FROM {args.source}"
        df = fetch_data_from_teradata(query)
    else:
        raise ValueError("Invalid data type. Choose 'csv' or 'table'.")
    
    # Load the data into BigQuery
    load_data_to_bigquery(df, args.project, args.dataset, args.table)

if __name__ == "__main__":
    main()
