+++
title = 'Data Processing'
date = 2024-10-04T20:42:44-04:00
draft = false
+++

Let's process some data with Python and Pandas!

Let's say we are keeping track of donations made but we only want the names and the type of donations BUT we have an undetermined number of excel files that has a lot more than we need, what do we do?!

We use pandas of course!
- Provide the names of the columns we want
- Let's say that we have a weird space in a column name but we can fix that no problem.
    - Let's have our current column names as one variable
    - Let's have another variable with proper column names
- We then want to split the donation types.

Let's begin!
- First we are going to make a process data function that takes in the file path
    - This function will take in data from the provided excel file
    - We will rename the offending column
    - We then set the data frame to the values we care about
    - We will also drop empty cells because why do we need them?
    - Let's add an empty space back in so everything looks good
    - Finally let's drop the last row because that's always an empty space and return the data frame

- Next we need a process files in folder function that takes in
    - We need an empty data frame list
    - For every file in the folder path and if it is indeed a file we print out the file we are working with
    - We append the returned value from the process_data inside of the data frame list
    - Finally we return a concatenated list of data frames

- Finally we get to the actual processesing
    - We provide the path to the folder and store it as a variable
    - We set a list to the returned value of the concatenated data frame
    - We create an MList and HList that we drop duplicates
    - We sort any suffixes in order and inplace
    - We create a today object and set it to today's date
    - We use the formatted string with the to_csv method of dataframes to name the files that we are creating.

```python
import os
from datetime import datetime
import pandas as pd

start = datetime.now()

column_names = ['Donor(s) Name', 'Gift Type', 'Check Number', 'Amount',
       '     First Name', 'MI', 'Last Name', 'Suffix', 'Type', 'Fund  ',
       'Ack ']
desired_columns = ['First Name', 'MI', 'Last Name', 'Suffix', 'Type']


def process_data(file_path):
    """
    Processes a file. 
    Args:
        file_path: The path to the file.
    """
    df = pd.read_excel(file_path, header=5)
    df = df.rename(columns={'     First Name': 'First Name'})
    df = df[desired_columns]
    df = df.dropna(subset=['First Name', 'Last Name'], how='all')
    df = df.fillna(' ')
    df = df.iloc[:-1]
    return df

def process_files_in_folder(folder_path):
    """
    Processes files in a specified folder.
    Args:
        folder_path: The path to the folder.
    """
    df_list = []

    for filename in os.listdir(folder_path):
        file_path = os.path.join(folder_path, filename)
        if os.path.isfile(file_path):
            print(f"Processing file: {file_path}")
            df_list.append(process_data(file_path))
    return pd.concat(df_list)

folder_path = "C:\\Path\\To\\Folder"
list = process_files_in_folder(folder_path)

MList = list[list['Type'] == 'M']
HList = list[list['Type'] == 'H']

MList = MList.drop_duplicates(subset=['First Name', 'MI', 'Last Name', 'Suffix'])
HList = HList.drop_duplicates(subset=['First Name', 'MI', 'Last Name', 'Suffix'])

MList.sort_values(by=['First Name', 'MI', 'Last Name', 'Suffix'], inplace=True)
HList.sort_values(by=['First Name', 'MI', 'Last Name', 'Suffix'], inplace=True)

today = datetime.now()
today = today.strftime('%Y-%m-%d')
MList.to_csv(f'memorials-{today}.csv', index=False)
HList.to_csv(f'honorariums-{today}.csv', index=False)


duration = (datetime.now() - start).total_seconds()
print(f"Time taken {duration} seconds")

```

We should always look to automate the boring stuff, since the boring stuff usually takes forever and this can be much faster!