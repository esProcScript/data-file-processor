## Read and reorder specific columns on a CSV file

The format of the structured text is relatively standardized, that is, one record per line and the columns are separated by separators. SPL can read and write a structured text with import/export functions.

For example, ordersNT.csv stores order records. The business meanings are order ID, customer number, sales ID, order amount, and order date. Part of the data is as follows:

```
26,TAS,1,2142.4,2009-08-05
33,DSGC,1,613.2,2009-08-14
84,GC,1,88.5,2009-10-16
133,HU,1,1419.8,2010-12-12
32,JFS,3,468.0,2009-08-13
39,NR,3,3016.0,2010-08-21
43,KT,3,2169.0,2009-08-27
…,
```
Sort the table from small to large in alphabetical order of the customer number, and then sort the same customer number from large to small according to the order amount, and finally save the original format into a new file. Part of the results should be as follows:

```
136,ARO,25,899.0,2009-12-16
16,BDR,27,2464.8,2009-07-23
81,BDR,29,1168.0,2010-10-14
108,BDR,12,480.0,2010-11-15
139,BDR,30,166.0,2010-12-18
93,BON,6,2564.4,2010-10-29
106,BSF,27,10741.6,2009-11-13
…,
```

To calculate the above results, the following SPL script can be used:

| |A|
|:-|:-|
|1|=file("D:/data/ordersNT.csv").import()|
|2|=A1.sort(_2,-_4)|
|3|=file("D:/data/ordersNT_sort.txt").export(A2)|

SPL can also handle text files with column names (titles). For example, in the first row of orders.csv, the column names, some of the data are as follows:
```
OrderID,Client,SellerId,Amount,OrderDate
26,TAS,1,2142.4,2009-08-05
33,DSGC,1,613.2,2009-08-14
84,GC,1,88.5,2009-10-16
133,HU,1,1419.8,2010-12-12
32,JFS,3,468.0,2009-08-13
39,NR,3,3016.0,2010-08-21
43,KT,3,2169.0,2009-08-27
…
```
Sort the file in the same way, and write a new file with column names as a result. The following SPL script can be used:

| |A|
|:-|:-|
|1|=file("D:/data/orders.csv").import@t()|
|2|=A1.sort(Client,-Amount)|
|3|=file("D:/data/orders_sort.txt").export@t(A2)|

For more examples, see [Read and write CSV/XLS files](http://c.raqsoft.com/article/1616037803132) and know more about [esProc and SPL](http://www.raqsoft.com/p/script-over-csv-xls)


