# Data Manipulation with Python
-----------------------

This document contains my aggregated notes for data manipulation using Python and associated libraries.

## Table of Contents
- [Pandas](#pandas)
    - [Series](#series)
    - [DataFrames](#dataframes)
    - [Missing Data](#missing-data)
    - [Groupby](#groupby)
    - [Merging, Joining, and Concatenating](#merging-joining-and-concatenating)
    - [Misc. Operations](#misc-operations)
    - [Data Input and Output](#data-input-and-output)
- [BeautifulSoup](#BeautifulSoup)
- [sqlalchemy](#)



# Pandas

```py
import numpy as np
import pandas as pd
```
### Series
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
### DataFrames
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
df>0 # ...returns matrix of True/False values
df[df>0] # ...returns values for True and NaN for False
df[df['col1'] > 0][['col2','col3']]
# ...returns col2 and col3 rows filtered by col1 > 0
# For two or more conditions, use | 'or' or & 'and'...
df[(df['col1'] > 0) & (df['col2'] > 1)]

# Resetting an index to the default...
df.reset_index()
# ...or, resetting it to an existing column in the df...
df.reset_index('col_name',inplace=True)

# For multi-index DataFrames (i.e. a df with index hierarchy)
#   rows can be selected across multiple indices...
df.loc['outer_index_label'].loc['inner_index_label']
# or, we can use cross-section 'xs'...
df.xs(['outer_index_label','inner_index_label'])

# Naming multi-index columns...
df.index.names = ['outer_index_name','inner_index_name']
# Then index names can be used in 'xs' command...
df.xs('inner_index_label',index='inner_index_name')
```
### Missing Data
```py
# Find Null values...
df.isnull() # Returns df with boolean values True or False
# dropping NaN values from Pandas df...
df.dropna(axis=0, thresh=0, inplace=True, ...)
# Filling NaN values...
df.fillna(value='fill_value(s)')
# Example of filling NaN values of a particular column
#   with the mean value for the column...
df['col_name'].fillna(value=df['col_name'].mean())
```
### Groupby
.groupby() method creates DataFrameGrouBy object that groups rows of data and allows you to call aggregate functions
```py
# Simple example...
df2 = df.groupby('col_name')
df2.mean()
# ...or, just to print with single code line...
df.grouby('col_name').mean()

# Sample aggregate functions...
df2.mean()
df2.std()
df2.min()
df2.max()
df2.count()
df2.describe() # Gives summary stats for each variable
df2.transpose()
# Can be more complex single line commands...
df2.describe().transpose()['variable_name']
```
### Merging Joining and Concatenating
Three main methods for combining DataFrames together.
```py
# Concatenating multiple DataFrames...
pd.concat([df1,df2,df3],axis=0)

# Merging multiple DataFrames...
pd.merge(df_left,df_right,how='inner',on='col_name')
# Can also merge on multiple key columns...
pd.merge(df_left,df_right,how='right',on=['col1','col1'])

# Joining multiple DataFrames allows easy combination of
#   two differently-indexed DataFrames into a single DF.
df1.join(df2, how='outer')

# Merging is done off columns and Joining is done on index!
```
### Misc. Operations
```py
# List unique values...
df['col_name'].unique()
# Count number unique values...
df['col_name'].nunique()
# Count frequency of each unique value...
df['col_name'].value_counts()

# Applying functions...
def func_name(x):
  return some_function_with_x

df['col_name'].apply(func_name)

# Can also be accomplished with Lambda expressions...
df['col_name'].apply(lambda x: some_function_with_x)

# Apply can be used other ways as well.
# For instance, to count number of characters in each row of a col...
df['col_name'].apply(len)

# Summarizing a column based on an aggregate function...
df['col_name'].mean()

# Print column and index names...
df.columns
df.index

# Sorting a DataFrame
df.sort_values(by='col_name', inplace=False)

# Create a Pivot Table similar to Excel...
df.pivot_table(values='col1',
               index=['col2','col3'],
               columns=['col3'])

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
