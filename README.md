def extract_text_between_texts(target_text):
	file_path = ‘ c/Users/vudumli/Desktop/Bill_details/code/bill.pdf’
	extracted_text = “”
	found_target = False
	with open(file_path,’rb’) as file:
		reader = PyPDF2.pdfReader(file)
		num_pages = len(reader.pages)

		for page_num in range(num_pages):
			page = reader.pages[page_num]
			text = page.extract_text()
			
			if target_text in text:
				found_target =True
   
			if found_target:
				extracted_text+ =text
			
			if target_text in text and found_target:
				found_target = False 
		pattern = r”\$\d+(?:,\d+)*(?:\.\d+)?”
		dollar_values = re.findall(pattern , extracted_text)
		bill_values = pd.DatFrame(dollar_values[:-2]).T
	return bill_values
  
  
  def extract_text_between_texts(target_text):
  

  # Open the PDF file.
  with pdfplumber.open('bill.pdf') as pdf:

    # Find the first occurrence of the target text.
    first_occurrence = pdf.find_text(target_text)

    # Find the next occurrence of the target text.
    next_occurrence = pdf.find_text(target_text, after=first_occurrence)

    # Extract the text between the two occurrences of the target text.
    extracted_text = pdf.pages[first_occurrence:next_occurrence].extract_text()

    # Use a regular expression to extract the dollar values from the extracted text.
    pattern = r”\$\d+(?:,\d+)*(?:\.\d+)?”
    dollar_values = re.findall(pattern, extracted_text)

    return dollar_values
    
    
    import pdfplumber
import re
import pandas as pd

def extract_text_between_texts(target_text):
    file_path = 'c/Users/vudumli/Desktop/Bill_details/code/bill.pdf'
    extracted_text = ""
    found_target = False
    
    with pdfplumber.open(file_path) as pdf:
        num_pages = len(pdf.pages)

        for page_num in range(num_pages):
            page = pdf.pages[page_num]
            text = page.extract_text()

            if target_text in text:
                found_target = True

            if found_target:
                extracted_text += text

            if target_text in text and found_target:
                found_target = False

    pattern = r"\$\d+(?:,\d+)*(?:\.\d+)?"
    dollar_values = re.findall(pattern, extracted_text)
    bill_values = pd.DataFrame(dollar_values[:-2]).T
    
    return bill_values
    
    
    import mysql.connector

def insert_into_mysql_db(host, user, password, database, data):
    try:
        # Connect to the MySQL database
        conn = mysql.connector.connect(
            host=host,
            user=user,
            password=password,
            database=database
        )
        
        # Create a cursor object to execute SQL queries
        cursor = conn.cursor()
        
        # SQL query to insert data into the table
        sql = "INSERT INTO your_table_name (column1, column2, column3) VALUES (%s, %s, %s)"
        
        # Execute the query for each set of data
        for item in data:
            # Assuming data is a list of tuples, modify this part based on your data structure
            cursor.execute(sql, item)
        
        # Commit the changes to the database
        conn.commit()
        
        print("Data inserted successfully into MySQL database")
        
    except mysql.connector.Error as error:
        print(f"Error inserting data into MySQL database: {error}")
        
    finally:
        # Close the cursor and database connection
        if conn.is_connected():
            cursor.close()
            conn.close()
            print("MySQL connection is closed")

