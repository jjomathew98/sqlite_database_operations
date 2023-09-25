# sqlite_database_operations
## For Step 1: Data Exploration and Analysis
We imported two Pricing Transparency Datasets (https://raw.githubusercontent.com/hantswilliams/HHA_504_2023/main/WK3/data/nyp/133957095_NewYorkPresbyterianHospital_standardcharges.json and https://raw.githubusercontent.com/hantswilliams/HHA_504_2023/main/WK3/data/stonybrook/stonybrook.csv

## Imported the datasets into Pandas DataFrames using the pandas.read_csv() function.

df1 = pd.read_json('https://raw.githubusercontent.com/hantswilliams/HHA_504_2023/main/WK3/data/nyp/133957095_NewYorkPresbyterianHospital_standardcharges.json')
df2 = pd.read_csv('https://raw.githubusercontent.com/hantswilliams/HHA_504_2023/main/WK3/data/stonybrook/stonybrook.csv')

## To conduct a Basic Exploratory Analysis:

### To explore the datasets we used:
df.head(): View the first few rows of the DataFrame.
df.info(): Get information about the DataFrame, including data types and missing values.
df.describe(): Get basic statistics for numerical columns.
df['1199'].value_counts(): Get frequency counts for categorical columns.

df1.head()
df1.info()
df1.describe()
df1['1199'].value_counts()

# Step 2: SQLite Database Operations (df1 and df2)

2.1 Create SQLite Database
2.2 Manual Table Creation and Data Insertion
2.3 Automatic Table Creation

For these purpose we used the following codes for:

import sqlite3

# 2.1 Create SQLite Database
conn = sqlite3.connect('health.db')

# 2.2 Manual Table Creation and Data Insertion
cursor = conn.cursor()

# Create a table
create_table_query = '''
CREATE TABLE IF NOT EXISTS hospital_data (
    id INTEGER PRIMARY KEY,
    numerical_col1 REAL,
    numerical_col2 REAL,
    categorical_col1 TEXT,
    categorical_col2 TEXT
);
'''
cursor.execute(create_table_query)

# Insert data into the table
insert_data_query = '''
INSERT INTO hospital_data (numerical_col1, numerical_col2, categorical_col1, categorical_col2)
VALUES (?, ?, ?, ?);
'''
cursor.execute(insert_data_query, (1.0, 2.0, 'Category1', 'Category2'))
cursor.execute(insert_data_query, (3.0, 4.0, 'Category3', 'Category4'))

# Commit changes and close the connection
conn.commit()
conn.close()

# 2.3 Automatic Table Creation

conn = sqlite3.connect('health.db')
df1.to_sql('hospital_data_auto', conn, if_exists='replace', index=False)
conn.close()

