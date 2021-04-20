# Compare two CSV files and find common or different data

For Example,there are two text files. Each row of both files is a string. We want to compare them row by row. To do this, we read in each row of every file as a string to form a set of strings and then perform a set operation.

paint.txt and dance.txt record IDs and names of children who enrolled in painting class and dancing class respectively. Below is part of paint.txt:
```
  20121102-Joan
  20121107-Jack
  20121113-Mike
```
- Find common data

To find common records of the two files is to get their intersection.

Example: Find all children who enrolled in both painting class and dancing class and write their records to p_d.txt.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read each of the rows from paint.txt to form a set|
|2|=file("e:/txt/dance.txt").read@n()|Read each of the rows from dance.txt to form a set|
|3|>file("d:/result.xlsx").xlsexport(A1\^A2;"common")|Write their intersection to result.xlsx|

- Find differences

Find all different records of the two files.

Example: Find the children who enrolled in either painting class or dancing class.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read each of the rows from paint.txt to form a set|
|2|=file("e:/txt/dance.txt").read@n()|Read each of the rows from dance.txt to form a set|
|3|>file("d:/result.xlsx").xlsexport@t(A1%A2;"differences")|Write their XORs to result.xlsx|


Comparing two text files to find their common or different data is not a rare thing in data analysis work. According to the content to be compared, there are row-wise comparison and comparison by key field(s); and according to the sizes of files under comparison, there are small files comparison and big files comparison.
For more details, see [Samples of Comparing Files](http://c.raqsoft.com/article/1600309188122)
