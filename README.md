# Data-Cleaning-Project  
  This repository contains a Jupyter notebook that processes raw data from an Excel file (TASK -- convert raw data - clean data.xlsx) and transforms it into a clean, structured dataset.      The notebook performs data cleaning tasks such as extracting address components, converting date and time columns, and handling missing data.  
**Features**  
Address Data Processing:  
  The notebook extracts address components (HouseNO, Locality, City, PIN) from a single ADDR column using regular expressions.  
Date and Time Processing:  
  Converts the DT column into a Date column and extracts Year, Month, and Day.  
  Splits the time column into separate Hour, Minute, and Second columns.  
Data Cleaning:  
  Drops unnecessary columns (ADDR, DT, and time).  
  Removes columns with missing data to ensure the dataset is complete and ready for analysis.  
**Data Cleaning Workflow**  
1. Loading the Data  
    The dataset is loaded from the Excel file TASK -- convert raw data - clean data.xlsx using pandas.read_excel(). The first two rows are skipped to ensure proper header alignment:  
        import pandas as pd  
        df = pd.read_excel(r'C:\path\to\TASK -- convert raw data - clean data.xlsx', skiprows=2)  
        df.head()  # Displays the first few rows of the dataset  
2. Processing the Address Column  
    The ADDR column, which contains the address information in a single string, is split into four new columns: HouseNO, Locality, City, and PIN. This is done using regular expressions:  
        df[['HouseNO', 'Locality', 'City', 'PIN']] = df['ADDR'].str.extract(r'^(.*?),\s*(.*?),\s*(.*?)(?:,?\s*(\d{6}))?$')  
        df.head()  # Displays the first few rows of the dataset after extraction  
3. Date Conversion  
    The DT column is converted into a Date column, and additional columns for Year, Month, and Day are extracted:  
          df['Date'] = pd.to_datetime(df['DT']).dt.date  
          df['Year'] = pd.to_datetime(df['Date']).dt.year  
          df['Month'] = pd.to_datetime(df['Date']).dt.month  
          df['Day'] = pd.to_datetime(df['Date']).dt.day  
   df.head()  # Displays the first few rows with the new date columns  
4. Time Data Parsing  
      The time column is split into three new columns: Hour, Minute, and Second:  
                df[['Hour', 'Minute', 'Second']] = df['time'].str.split(':', expand=True)  
                df.head()  # Displays the first few rows with the new time columns  
5. Data Cleanup  
    The following columns are dropped: ADDR, DT, and time. This step ensures that only relevant data remains in the dataset:  
          df = df.drop(['ADDR', 'DT', 'time'], axis=1)  
          df.head()  # Displays the cleaned dataset  
6. Removing Missing Data  
    Columns containing missing data are dropped:  
      df = df.dropna(axis=1)  
      df.head()  # Displays the dataset with missing columns removed  
    
**Outputs**   
Cleaned Data: After running the notebook, the raw dataset will be cleaned and structured, with separate columns for address components, formatted dates, and time data.  
Statistical Summaries: The cleaned dataset can be further analyzed or visualized.  
      Example of Data Exploration Output  
          After running the notebook, you’ll get a cleaned DataFrame, which you can explore using standard Pandas functions like:  
                df.head()  # Displays the first few rows of the cleaned dataset  
          You can also save the cleaned data to a new Excel or CSV file:  
                df.to_csv('cleaned_data.csv', index=False)  # Save to CSV
