## Filter and summary a large CSV file

Structured text file processing, such as TXT and CSV files, is common data analysis work. Sometimes the file is too big to be loaded into the memory at one time. You need to load and calculate it in batches and then aggregate the temporary intermediate result sets according to the specific requirements. The whole process is quite different from that of calculating a small file that can be wholly loaded into the memory.

Maybe you are  familiar with the database cursor. A database cursor returns a part of the data each time rather than load all data to the memory at one time. A file cursor in esProc works similarly in reading data from a big text file. esProc is the SPL-based professional data computing engine equipped with all-round cursor objects and operations. It’s convenient to handle filtering, sorting, aggregate with esProc.

Example: students_scores.csv is a big file that records student scores. Below is part  of the file:

```
CLASS,NAME,English,Chinese,Math
1,Adams Brooke,63,31,69
1,Adams Hannah,89,85,79
1,Adams Jonathan,88,87,91
1,Allen Ashley,98,97,97
```

## Filtering

Find scores of students of class 10.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/students_scores.csv").cursor@tc()|Use @t option to read the first row as column headers|
|2|=A1.select(CLASS==10)|Select scores of students of class 10, which is a delayed calculation|
|3|=file("E:/txt/students_scores_10.csv").export@t(A2)|Export the eligible records to a new file|

## Sorting

Sort the CSV file students_scores.csv that records student scores by Chinese score in ascending order:

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/students_scores.csv").cursor@tc()|Create a cursor|
|2|=A1.sortx(Chinese)|Sort records by Chinese in ascending order and return a cursor|
|3|=file("E:/txt/students_scores_sort.txt").export@t(A2)|Export the sorted records to a new file|

You can sort records by multiple fields or an expression. To change A2’s expression in the following way, for example:

```
=A1.sortx(Chinese,Math) // Sort by Chinese and then Math

=A1.sortx(Math+English+Chinese) // Sort by total score
```



## Aggregate

An aggregate operation summarizes values of a specified column over all records in a big text file. There are sum, average, max, min and count, etc. To decrease memory use, we traverse all records in the cursor, calculate the current aggregate over each record and store only each aggregate result instead of all records in the memory. We’ll get the final result as soon as the traversal is over.

To calculate the total Chinese scores, esProc SPL produces the following script:

|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/students_scores.csv").cursor@tc()|@c means using comma as the separator|
|2|=A1.total(sum(Chinese))|Sum the Chinese scores|

To calculate the total Chinese scores in class 10, esProc SPL has the following script:
|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/students_scores.csv").cursor@tc()|@c means using comma as the separator|
|2|=A1.select(CLASS==10).total(sum(Chinese))|Get records of class 10 and sum its   Chinese scores|


## Grouping & aggregation

Example: The big file user_info_reg.csv records user login information. We want to calculate the total number and duration of user logins in each province. Values of different columns are separated by commas. Below is part of the file:

```
user_id,reg_mon,age,cell_province,id_province,id_city,insertdate,reg_time
483833,2017-04,19,c29,c26,c26241,2018-12-11,56558
156772,2016-05,31,c11,c11,c11159,2018-02-13,81617
173388,2016-05,34,c02,c02,c02182,2018-08-21,729
199107,2016-07,25,c09,c09,c09046,201 8-06-05,86299
122560,2016-03,23,c05,c05,c05193,2018-04-02,2657
550399,2017-06,30,c08,c07,c07282,2018-04-19,54413
414650,2017-02,53,c12,c12,N,2018-10-16,57077
275042,2016-10,23,c21,c21,c21174,2018-07-10,69604
184180,201 6-06,29,c04,c04,c04348,2018-02-08,18890
```

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/user_info_reg.csv").cursor@tc()|Create a cursor; @c option enables using commas as the separator|
|2|=A1.groups(id_province;count(\~):cnt,sum(reg_time):total_reg)|Group records and calculate the number and duration of user logins in each province|



For detail explanation, see [Samples of Processing Big Text File](http://c.raqsoft.com/article/1599117027835) and know more about [esProc and SPL](http://www.raqsoft.com/p/script-over-csv-xls)

