# Calculate a huge text file

A large file is too large to be read in at one time because of insufficient computer memory. In this case, direct use of desktop data tools (such as Excel) is powerless and often needs to write a program to deal with it. Even if the program is written, a large file must be read in batches for calculation and processing. Finally, the batch processing results need to be properly summarized according to different calculation types, which is much more complicated than the small file processing.

Python does not have a big problem with small files, but it does not provide effective support for large files. Compared with the high-level language, the reduced workload is very limited, and the usability is not high.

esProc is a professional data processing tool. Like the database, it has built-in query and calculation algorithms. It can directly use files such as text, Excel, XML, JSON, to calculate, without the process of importing data.

esProc provides a cursor, which can read data in batches and then calculate, so it is convenient to process large files. For example, you only need one line of code:
1. Calculate total order amount
```
=file(“orders.txt”).cursor@t().total(sum(amount))
```

It is also very easy if you want to add a filtering. For example, you can only count the total order amount since 2009:
```
=file(“orders.txt”).cursor@t().select(orderdate>=date(“2009-01-01”)).total(sum(amount))
```

2. Grouping and sorting are also simple：Group by state and calculate total order amount by state
```
=file(“orders.txt”).cursor@t().groups(state;sum(amount))
```

3. Sort by order amount
```
=file(“orders.txt”).cursor@t().sortx(amount)   
```

esProc even allows you to query files directly using SQL. For example:
```
$select sum(amount) from "orders.txt"
$select state, sum(amount) from "orders.txt" group by state
$select * from "orders.txt" order by amount
```

esProc also has built-in parallel computing and can use multi-core CPU to improve performance, which is particularly useful for large files.
For example, group aggregate calculation is written as follows:
```
=file("orders.txt").cursor@tm(;4).groups(state;sum(amount))
```

It will be calculated in 4-way parallel mode, and the speed can be increased by 2–3 times on an ordinary multi-core laptop.
The key point is that it is used for free.

For more details, see [Samples of Processing Big Text File](http://c.raqsoft.com/article/1599117027835) and know more about [esProc and SPL](http://www.raqsoft.com/p/script-over-csv-xls)

