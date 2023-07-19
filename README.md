import os
import csv

# Path to the parent folder containing all the folders
parent_folder = 'path_to_parent_folder'

consolidated_file_path = 'path_to_save_consolidated_file.csv'

# Open the consolidated file in write mode
with open(consolidated_file_path, 'w', newline='') as consolidated_file:
    writer = csv.writer(consolidated_file)

    # Iterate over each folder
    for folder_name in os.listdir(parent_folder):
        folder_path = os.path.join(parent_folder, folder_name)

        # Check if the path is a directory
        if os.path.isdir(folder_path):
            # Iterate over each file in the folder
            for file_name in os.listdir(folder_path):
                file_path = os.path.join(folder_path, file_name)

                # Check if the file is a CSV file
                if file_name.endswith('.csv'):
                    # Open the CSV file and read its contents
                    with open(file_path, 'r') as csv_file:
                        reader = csv.reader(csv_file)
                        file_data = list(reader)

                    # Write the contents of the CSV file to the consolidated file
                    writer.writerows(file_data)

print("Consolidation complete!")
