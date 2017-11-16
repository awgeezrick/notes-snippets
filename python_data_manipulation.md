# Data Manipulation with Python - Notes and snippets
-----------------------

This document contains my aggregated notes for data manipulation using Python and associated libraries.

## Table of Contents
- [Pandas](#Pandas)
- [BeautifulSoup](#BeautifulSoup)
- [sqlalchemy](#)



# Pandas

```py
import numpy as np
import pandas as pd
```
### Pandas Series
Similar to a NumPy array, but a Series...
1. Can have axis labels
1. Does not need to hold numeric data, and can hold an arbitrary Python Object

```py
# Creating a series from two lists...
pd.Series(data='list_name1',index='list_name2')
# ...or from a list and a NumPy array...
pd.Series(data='array_name',index='list_name')
# ...or from a dictionary containing both labels and items...
pd.Series(data='dict_name')

# Grabbing information from a Series...
series_name['index_label']
```
### Pandas DataFrames
Similar to a bunch of Series objects put together to share a single index.

```py
# Creating a DataFrame...
df = pd.DataFrame(data='data_object',
                  index='index_object',
                  columns='columns_object')

# Selecting columns...
df['col_name']
df[['col_name1','col_name2',...]]
df['col_list']
# Creating a new column...
df['new_column'] = df['col1'] + df['col2']
# Removing columns...
df.drop('col_name',axis=1,inplace=True)

# Selecting Rows...
df.loc['label_name']
df.loc['label1','label2']
df.loc[['label1','label2'],['col1','col2']]
# ...or based on index position instead of label...
df.iloc[position_number]
# Removing rows...
df.drop('label_name',axis=0,inplace=True)

# Conditional Selection...
# Two or more conditions...

```
### Data Input and Output
```py
# CSV Input
df = pd.read_csv('filename')

# CSV Output
df.to_csv('filename',index=False)
# ...index=False does not save our old index.
#   Otherwise, it is saved as unamed column and reindexed

# Excel Input
pd.read_excel('filename.xlsx',sheetname='sheetname')

# Excel Output
df.to_excel('filename.xlsx',sheet_name='sheetname')
# If passing an existing ExcelWriter object, each sheet is
#   added to the existing workbook.
# This can save different DataFrames to one workbook...
writer = pd.ExcelWriter('output.xlsx')
df1.to_excel(writer,'Sheet1')
df2.to_excel(writer,'Sheet2')
writer.save()

# HTML Input
df = pd.read_html('urlname')
df[0]
```
