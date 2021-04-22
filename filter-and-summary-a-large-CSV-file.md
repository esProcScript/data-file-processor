## Filter and summary a large CSV file

Structured text file processing, such as TXT and CSV files, is common data analysis work. Sometimes the file is too big to be loaded into the memory at one time. You need to load and calculate it in batches and then aggregate the temporary intermediate result sets according to the specific requirements. The whole process is quite different from that of calculating a small file that can be wholly loaded into the memory.

Maybe you are  familiar with the database cursor. A database cursor returns a part of the data each time rather than load all data to the memory at one time. A file cursor in esProc works similarly in reading data from a big text file. esProc is the SPL-based professional data computing engine equipped with all-round cursor objects and operations. It’s convenient to handle filtering, sorting, aggregate with esProc.

Example: students_scores.txt is a big file that records student scores. Below is part  of the file, values of different columns are separated by tabs.

```
CLASS	NAME	English	Chinese	Math
1	Adams Brooke	63	31	69
1	Adams Hannah	89	85	79
1	Adams Jonathan	88	87	91
1	Allen Ashley	98	97	97
```

## Filtering

Find scores of students of class 10 from it. 

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("E:/txt/students_scores.txt").cursor@t()|Use @t option to read the first row as   column headers|
|2|=A1.select(CLASS==10)|Select scores of students of class 10,   which is a delayed calculation|
|3|=file("E:/txt/students_scores_10.txt").export@t(A2)|Export the eligible records to a new   file|

## Aggregate

An aggregate operation summarizes values of a specified column over all records in a big text file. There are sum, average, max, min and count, etc. To decrease memory use, we traverse all records in the cursor, calculate the current aggregate over each record and store only each aggregate result instead of all records in the memory. We’ll get the final result as soon as the traversal is over.

Example: students_scores.csv is a big file that records student scores. Values of different columns are separated by commas. Below is part of the file:


