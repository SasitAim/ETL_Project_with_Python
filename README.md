# ETL_Project_with_Python
ETL data from a Kaggle public dataset.

### Project Overview
In this project, will use data from Kaggle to perform data cleaning and obtain the desired dataset. Once the cleaning process is completed, we will proceed to connect to the database and upload the data into the MySQL database, which has been created the 'airline' database.

### Tool
  - Jupyter Notebook & Python
  - MySQL

### The Data Cleaning steps for this project include:
1. Import from a Kaggle public dataset.
2. Cleaning data.
3. Connect to MySQL database and upload data after finising data cleaning process.


### Import library
```python
# import library
import opendatasets as od
import os
import pandas as pd
```

### Import data from Kaggle
```python
# identify the dataset's URL on Kaggle
url_data = 'https://www.kaggle.com/datasets/iamsouravbanerjee/airline-dataset'
```

```python
# download dataset form kaggle by identify URL of dataset 
# used Kaggle AIP username and key
od.download(url_data)
```

```python
# identify directory for save data 
data_dir = './airline-dataset'
data_dir
```

```python
# download file in directory 
# output is file name 
os.listdir(data_dir)

```

### Create data frame
```python
# create data frame
file_path = data_dir + '\Airline Dataset Updated.csv' # file_path = data_dir + 'file name'
df = pd.read_csv(file_path)
df
```

### Overview of Data
```python
# show first 5 rows of data
df.head()
```

```python
# Check information of data as number of rows and columns, all column name, data type and Non-Null Count 
df.info()
```

```python
# Check missing value in dataset
df.isnull().sum()
```

```python
# Show statistics about data (show only numeric data)
df.describe()
```

### Data Cleaning
```python
# Cleaning data
# separate day and month from Departure Date
df['Departure Date'] = pd.to_datetime(df['Departure Date']) # convert column 'Departure Date' to datetime
df['Day_of_week'] = df['Departure Date'].dt.dayofweek # add column 'Day_of_Week' by .dt.dayofweek
df['Month'] = df['Departure Date'].dt.month
```

```python
df.info()
```

```python
# Check data after clean
df.head()
```

```python
# save data frame to csv file
df.to_csv('df_airline.csv', index=False)
```

### Connect to MySQL
```python
# Import the neccessary modules
from sqlalchemy import create_engine as ce
```

```python
# Detail for connect MySQL
host = 'localhost' #localhost
user = 'root' # user name
password = 'rootuser' # Your password
database = 'datasciencedb' # database name
```

```python
# Create connection
engine = ce(f'mysql+pymysql://{user}:{password}@{host}/{database}')
```

```python
# Import DataFrame DB
df.to_sql(name='airline', con=engine, if_exists='replace', index=False)host = 'localhost'
user = 'root'
password = 'rootuser'
database = 'datasciencedb'
```

```python
# close connections
engine.dispose()
```
