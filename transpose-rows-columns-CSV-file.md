# Transpose rows and columns of a CSV file



## Transpose rows and columns

Transpose is to realize the conversion between rows and columns, that is, the values in the rows are used as column names.

Example: Sales table classified by channel, recorded by date. Part of the data is as follows:

```
YEAR,MONTH,ONLINE,STORE
2020,1,2440,3746.2
2020,2,1863.4,448.0
2020,3,1813.0,624.8
2020,4,670.8,2464.8
2020,5,3730.0,724.5
...
```
Expect to get the following format result:

```
CATEGORY,1,2,3,…
ONLINE,2440,1863.4,1813.0,…
STORE,3746.2,448.0,624.8,…
```
esProc SPL script:

| |A|
|:-|:-|
|1|=T("MonthSales.csv").select(YEAR:2020)|
|2|=A1.pivot@r(YEAR,MONTH; CATEGORY, AMOUNT)|
|3|=A2.pivot(CATEGORY; MONTH, AMOUNT)|

Use the function A.pivot@r() to convert rows and A.pivot() to convert rows to columns according to the logical sequence.


## Aggregate and transpose

The actual application of PIVOT basically follows the grouping and aggregation operation, after processing each row of data in the transposed column into a unique value through grouping, and then expanding the value of each row as the column name.

```
CLASS,STUDENTID,SUBJECT,SCORE
1,1,English,84
1,1,Math,77
1,1,PE,69
1,2,English,81
1,2,Math,80
...,...,...,...
```
Expect to get the following format result:

```
CLASS,MAX_MATH,MAX_ENGLISH,MAX_PE
1,97,96,97
2,97,96,97
…,…,…,…
```
esProc SPL script:

| |A|
|:-|:-|
|1|=T("Scores.csv")|
|2|=A1.groups(CLASS,SUBJECT;   max(SCORE):MAX_SCORE)|
|3|=A2.pivot(CLASS; SUBJECT, MAX_SCORE;   "Math":"MAX_MATH",   "English":"MAX_ENGLISH",   "PE":"MAX_PE")|



