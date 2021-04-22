# Remove duplicates from a CSV file

Sometimes during data processing we need to remove duplicate data from a file or retain only duplicates in a file. The methods include row-wise deletion or deletion by key field.The esProc SPL language it uses boasts a set of all-round function libraries for handling set-based operations. That makes it easy and simple to perform distinct over files.

## Row-wise DISTINCT

In a text file, each row is a string. We want to retain only one of each set of duplicate rows. To do this, we read in each row in the file as string to form a set of strings and perform distinct over the set.

Example: paint.txt records the IDs and names of students who register in the painting class. Some students have registered more than once, so we need to delete the extra duplicate records and write data to paint1.txt. Below is part of paint.txt:
```
  20121102-Joan
  20121107-Jack
  20121113-Mike
  20121107-Jack
```
esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/paint.txt").read@n()|Read in each row of paint.txt to form a set|
|2|=A1.id()|Delete duplicates from A1’s set|
|3|=file("e:/txt/paint1.txt").write(A2)|Write A2, the deduplicated set, into paint1.txt|

## Comparison by key field

We have a file that has columns of data. The first row contains column headers and detailed data starts from the second row. We want to compare values in the key field, and delete rows containing duplicate key values or retain them only.

Below is a part of the orders table of 2018 (order_2018.xlsx):

|+|A|B|C|D|E|
|:-|:-|:-|:-|:-|:-|
|1|ID|CustomerId|ProductId|OrderDate|Amount|
|2|10248|20108|1|2018-7-4|428.00|
|3|10249|20806|3|2018-7-5|1842.00|
|4|10250|30712|2|2018-7-8|1523.49|
|5|10251|10588|4|2018-7-8|624.94|
|6|10252|20108|1|2018-7-9|3559.49|
|7|10253|20806|2|2018-7-10|1428.00|

**Removing duplicates**

Example 1: Get the unique IDs of all customers that place an order in 2018 and write them into 2018c.xlsx.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/order_2018.xlsx").xlsimport@t()|Import orders data of 2018|
|2|=A1.id(CustomerId)|Get IDs of all different customers|
|3|=file("e:/txt/2018c.xlsx").xlsexport(A2)|Export unique IDs to 2018c.xlsx|

Example 2: Find the different products each customer bought in 2018, and write key fields CustomerId and ProductId to 2018c_p.xlsx.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/order_2018.xlsx").xlsimport@t(CustomerId,ProductId)|Import key fields of orders table of 2018|
|2|=A1.group@1(CustomerId,ProductId)|Group by the key fields; @1 option enables getting the first record of each group only|
|3|=file("e:/txt/2018c_p.xlsx").xlsexport@t(A2)|Export A2’s result to 2018c_p.xlsx|

**Retaining duplicates only**

Example: Get records where the customer bought same product many times and export them to 2018c_rebuy.xlsx.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file("e:/txt/order_2018.xlsx").xlsimport@t()|Import orders data of 2018|
|2|=A1.group(CustomerId,ProductId)|Put records where same customer buys same product into one group|
|3|=A2.select(\~.count()>1).conj()|Get groups where the number of records is greater than 1 and concatenate their records together|
|4|=file("e:/txt/2018c_rebuy.xlsx").xlsexport@t(A3)|Export A3’s result to 2018c_rebuy.xlsx|


The above script is written on the condition that a csv file is not too big. If it is not, look at how we can get  done over big files [Sample Programs of Performing Distinct on a File](http://c.raqsoft.com/article/1600308846480)



