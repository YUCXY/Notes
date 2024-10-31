Skills:
- [Iterate by row](#Iterate-by-row)
- [Append row to dataframe](#Append-row-to-dataframe)
- [Get value from column based on value from another column](#Get-value-from-column-based-on-value-from-another-column)
- [Get not intersection values of 2 lists](#Get-not-intersection-values-of-2-lists)
- [Sort table based on column values](#Sort-table-based-on-column-values)
- [Filter table based on length of values in column](#Filter-table-based-on-length-of-values-in-column)
- [Update column value while iterating](#Update-column-value-while-iterating)
- [Regex for data filtering](#Regex-for-data-filtering)
- [Check that string begins with certain character](#Check-that-string-begins-with-certain-character)
- [Drop rows of table with duplicates in certain column](#Drop-rows-of-table-with-duplicates-in-certain-column)
- [Get max from column](#Get-max-from-column)
- [Check if value is null/empty](#Check-if-value-is-null/empty)
- [Filter for column values between certain range](#Filter-for-column-values-between-certain-range)
- [Count groups in columns of table](#Count-groups-in-columns-of-table)
- [Aggregation](#Aggregation)
- [Merging tables](#Merging-tables)
- [Count number of each value in specific column](#Count-number-of-each-value-in-specific-column)
- [Efficiency](#Efficiency)

<br>

### Iterate by row
``` python
for i, row in df.iterrows():
```  
*i* returns index/primary key, *row* returns entire row as series

### Append row to dataframe
``` python
df = df._append({'row1':val1, 'row2':val2, 'row3', val3}, ignore_index=True)
``` 
essentially appends dict -> series to dataframe

### Get value from column based on value from another column
``` python
lst = customers.loc[customers['id'] == value, 'name'].iloc[0]
```
returns value from *name* where *id* is a certain value  
``` python
lst = customers.loc[customers['id'] == value, 'name']
```
returns object class object of *name* where *id* is a certain value;
can also take conditions for multiple columns but logical operators must use /& instead of or/and

### Get not intersection values of 2 lists
``` python
a.symmetric_difference(b)
```
returns non-intersection values/symmetric difference between 2 sets of values, where *a* and *b* must be type Set

### Sort table based on column values
``` python
df.sort_values(by=['col1'])
```  
sorts *df* in ascending order by values in column *col1*  
``` python
df.sort_values(by=['col1'], ascending=False)
```  
sorts *df* in descending order by values in column *col1*

### Filter table based on length of values in column
``` python
df[df['col1'].map(len) == val]
```  
returns sub-table from original table *df* where values of *col1* are equal to certain value *val*

### Update column value while iterating
``` python
for i in df.index:
	df.at[i, 'col1'] = value
```
updates *col1* of row *i* with *value*

### Regex for data filtering
``` python
users[users['mail'].str.match(r'^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\.com
```)]
```

### Check that string begins with certain character
``` python
patients[patients['conditions'].str.contains(r'\bDIAB1')]
```
'\b' dictates beginning of string, 'r' allows '\b' condition

### Drop rows of table with duplicates in certain column
``` python
employee.drop_duplicates(subset=['salary'])
```
drops rows of *employee* table with duplicates in *salary* column

### Get max from column
``` python
df['salary'].max()
```
returns max value in *salary* column

### Check if value is null/empty
``` python
pd.isna(df.iloc[i]['price'])
```
returns whether price at row *i* is null/empty

### Filter for column values between certain range
``` python
df[df['salary'].between(x,y)]
```
returns table where salary lies between range *x*,*y*

### Count groups in columns of table
``` python
df.groupby('col1')['col2'].count()
```
groups table by values in *col1* and counts values in *col2*
``` python
df.groupby('col1')['col2'].count().reset_index()
```
turns counts into table where first column is unique values of *col1* and second column is counts of values from *col2* for each value of *col1*
``` python
df.groupby(['col1','col2'])
```
to group by multiple columns, input column name as list

### Aggregation
aggregation applies multiple operations over specified axis
``` python
activities.groupby('sell_date').agg(num_sold=('product','nunique'),products=('product',lambda x:','.join(sorted(set(x))))).reset_index().sort_values('sell_date',inplace=True)
```
aggregate takes *activities* table grouped by *sell_date* and applies aggregation operations *nunique* and sorted unique string list to form 2 new columns *num_sold* and *products*
``` python
num_sold=('product','nunique')
products=('product',lambda x:','.join(sorted(set(x))))
```
*num_sold* column applies the *nunique* function to the *product* column to get the number of unique products
*products* column applies the lambda function to join the set of products in the *product* column as a string list separated by commas

### Merging tables
``` python
employee_uni.merge(employees, how='right', on='id')
```
merges tables *employee_uni* and *employees* on the common column *id*, with keys taken from the *right* table, filling in null values for keys which don't have corresponding values in the other table; variations of *how*: left, right, inner, outer, cross

### Count number of each value in specific column
``` python
df.col1.value_counts().reset_index()
```
returns table with each unique value from *col1* in the first column with header *col1* and number each unique value appears in *df* in second column with header *count*

### Efficiency
- list faster than dataframe/arrays b/c native
