import pandas as pd

def merge_csv_files(file1, file2, join_columns):
    # Read CSV files into pandas DataFrames
    df1 = pd.read_csv(file1)
    df2 = pd.read_csv(file2)
    
    # Merge DataFrames using inner join on specified columns
    merged_df = pd.merge(df1, df2, on=join_columns, how='inner')
    
    # Return the merged DataFrame
    return merged_df

# Example usage
file1 = 'file1.csv'
file2 = 'file2.csv'
join_columns = ['column1', 'column2']  # Specify the columns to join on

merged_data = merge_csv_files(file1, file2, join_columns)

# Print the merged DataFrame
print(merged_data)
