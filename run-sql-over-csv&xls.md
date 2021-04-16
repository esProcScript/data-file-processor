# Run SQL over xls/csv

## How to Use

### Execute SQL in esProc

<img src="http://www.raqsoft.com/wp-content/themes/raqsoft2017-en/images/script-over-csv-xls/2.png" width="800" height="600">

### Execute SQL at command line

For Windows:

```esprocx -r select state, sum(amount) as sum_amount from d:/excel/orders.xlsx group by state```

<img src="http://img.raqsoft.com.cn/uploads/1206/1607243485000bfdb.png">

For Linux:

```esprocx -r select state, sum(amount) as sum_amount from d:/excel/orders.xlsx group by state```



## Examples

esProc supports most of the syntax in SQL92 standards. Then you can export the SQL query result to a new Excel file or text file. 

|　|A|B|
|:-|:-|:-|
|1|$select \* from  E:/txt/Students_scores.txt where CLASS=10|/Filtering|
|2|$select  avg(Chinese),max(Math),sum(English) from E:/txt/Students_scores.txt|/Aggregation|
|3|$select  \*,English+Chinese+Math as total_score from E:/txt/students_scores.txt|/Inter-column calculations|
|4|$select  \*, case when English>=60 then 'Pass' else 'Fail' end as English_evaluation  from E:/txt/students_scores.txt| /CASE statement|
|5|$select \*  from E:/txt/students_scores.txt order by CLASS,English+Chinese+Math desc|/Sorting|
|6|$select  top 3 \* from E:/txt/students_scores.txt order by English desc|/TOP-N|
|7|$select  CLASS,min(English),max(Chinese),sum(Math) from E:/txt/students_scores.txt  group by CLASS| /Grouping & aggregation|
|8|$select  CLASS,avg(English) as avg_En from E:/txt/students_scores.txt group by CLASS having  avg(English)<70|/Post-grouping filtering|
|9|$select  distinct CLASS from E:/txt/students_scores.txt|/Distinct|
|10|$select  count(distinct PID) from E:/txt/PRODUCT_SALE.txt|/Distinct Count|
|11|$select  PID,count(distinct DATE) as no_sdate from E:/txt/PRODUCT_SALE.txt group by  PID|/Grouping & Count Distinct|
|12|$select  sum(S.quantity\*P.Price) as total from E:/txt/Sales.txt as S  join E:/txt/Products.txt as P on S.productid=P.ID  where S.quantity<=10|/Two-table join query|
|13|$select  e.NAME as NAME from E:/txt/EMPLOYEE_J.txt as e <br>   join E:/txt/DEPARTMENT.txt as d on  e.DEPTID=d.DEPTID <br>   join E:/txt/STATE.txt as s on  e.STATEID=s.STATEID <br> where d.NAME='HR' and s.NAME='California'|/Multi-table join query|
|14|$select  e.NAME as ENAME from  E:/txt/EMPLOYEE.txt as e <br>   join E:/txt/DEPARTMENT.txt as d on  e.DEPT=d.NAME <br>   join E:/txt/EMPLOYEE.txt as emp on d.MANAGER=emp.EID <br> where  e.STATE='New York' and emp.STATE='California'|/Multi-table, multilevel join query|
|15|$select  emp.BIRTHDAY as BIRTHDAY,emp.DEPT as DEPT from E:/txt/DEPARTMENT.txt as dept <br>    join E:/txt/EMPLOYEE.txt emp <br>    on dept.MANAGER=emp.EID <br> where <br>  emp.BIRTHDAY=(select  max(BIRTHDAY) <br>       from ( select emp1.BIRTHDAY as BIRTHDAY <br>            from E:/txt/DEPARTMENT.txt as  dept1 <br>             join E:/txt/EMPLOYEE.txt as  emp1 <br>             on dept1.MANAGER=emp1.EID <br>           ) <br>        )|/Nested subquery|


For more **complex scenarios** , please refer to [How to Use SQL in esProc](http://c.raqsoft.com/article/1603680137640)
