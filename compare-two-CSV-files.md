Comparing two text files to find their common or different data is not a rare thing in data analysis work. According to the content to be compared, there are row-wise comparison and comparison by key field(s); and according to the sizes of files under comparison, there are small files comparison and big files comparison

For Example,there are two text files. Each row of both files is a string. We want to compare them row by row. To do this, we read in each row of every file as a string to form a set of strings and then perform a set operation.

paint.txt and dance.txt record IDs and names of children who enrolled in painting class and dancing class respectively. Below is part of paint.txt:
  20121102-Joan
  20121107-Jack
  20121113-Mike
  
- Find common data

To find common records of the two files is to get their intersection.

Example: Find all children who enrolled in both painting class and dancing class and write their records to p_d.txt.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read each of the rows from paint.txt to form a set|
|2|=file("e:/txt/dance.txt").read@n()|Read each of the rows from dance.txt to form a set|
|3|=file("e:/txt/p_d.txt").write(A1\^A2)|Write their intersection to p_d.txt|

- Find differences

There are two types of scenarios of getting differences:

1. Find all different records of the two files.

Example: Find the children who enrolled in either painting class or dancing class.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read each of the rows from paint.txt to form a set|
|2|=file("e:/txt/dance.txt").read@n()|Read each of the rows from dance.txt to form a set|
|3|=file("e:/txt/p_d.txt").write(A1%A2)|Write their XORs to p_d.txt|

2. Find records from each file that doesn’t exist in the other one.

Example: Find children who enrolled in painting class or dancing class only.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read each of the rows from paint.txt to form a set|
|2|=file("e:/txt/dance.txt").read@n()|Read each of the rows from dance.txt to form a set|
|3|=file("e:/txt/p_1.txt").write(A1\A2)|Remove records children from A1 that also exist in A2 to get children who only enrolled in painting class and write them to p_1.txt|
|4|=file("e:/txt/d_1.txt").write(A2\A1)|Remove records children from A2 that also exist in A1 to get children who only enrolled in dancing class and write them to d_1.txt|

For more details, see [Samples of Comparing Files](http://c.raqsoft.com/article/1600309188122)
