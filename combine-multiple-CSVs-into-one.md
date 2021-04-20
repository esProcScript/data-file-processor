
# Combine multiple CSVs into one

1. Text files of same structure

A directory contains a number of text files of same column headers and structure but with different number of rows and detailed data. We want to merge these files into a single file under one set of column headers.

Example: there are same-structure text files that record daily orders under e:/orders. Each file has column headers on the first row and detailed data starting from the second row, as shown below. We want to merge them into one file named orders.txt.

|ID|Company Area|OrderDate|Amount|
|:-|:-|:-|:-|
|10248|Shantai North|2012/7/4|428|
|10249|Dongdiwang East|2012/7/5|1842|
|10250|Shiyi North|2012/7/8|1523.49|
|10251|Qi angu East|2012/7/8|624.94|
|10252|Fuxing West|2012/7/9|3559.49|
|10253|Shiyi North|2012/7/10|1428|

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=directory@p("e:/orders/\*.txt")|Get full paths of all text files under e:/orders|
|2|=A1.conj(file(\~).import@t())|Merge data of all text files|
|3|=file("e:/orders.txt").export@t(A2)|Export merged data to orders.txt|

If e:/orders has subdirectories, the text files under each subdirectory need to be merged too. In such case, A1 can be rewritten as =directory@ps("e:/orders/*.txt"). @s option enables getting files under all subdirectories recursively.

2. Text files of similar structure

Sometimes the text files don’t have completely same structure. They may contain different number of columns or have columns in different order, though all share a number of common columns. To merge the common columns in all files into one file, we need to retrieve them from each file in a specified order.  

Example: In the previous e:/orders, all orders files have ID, Company, Area, OrderDate and Amount columns, with different orders though. Some files contain other columns. The task is to merge these five columns of each file and write the result to orders.txt.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=directory@p("e:/orders/\*.txt")|Get full paths of all text files under e:/orders|
|2|=A1.conj(file(\~).import@t(ID,Company,Area,OrderDate,Amount))|Retrieve the five columns from every file in the specified order and merge them|
|3|=file("e:/orders.txt").export@t(A2)|Export the merge result to orders.txt|



