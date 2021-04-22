# Associative calculation and query on CSV files

## Associative calculation
Perform correlation calculations on the data in the two data tables.

Example: The sales order information and product information are stored in two Excel files, respectively, and the sales amount of each order is calculated. The data structure of the two files is as follows:

<img scr="http://img.raqsoft.com.cn/uploads/0319/16161397740002e28.png">

|+|A|
|:-|:-|
|1|=T(“e:/orders/sales.xlsx”)|
|2|=T(“e:/orders/product.xlsx”).keys(ID)|
|3|=A1.join(ProductID,A2,Name,Price)|
|4|=A3.derive(Quantity\*Price:amount)|

A1 Read sales order data.

A2 Read product information data, set ID as the primary key  

A3 associates A1 with the primary key in A2 according to ProductID , and at the same time takes out Name and Price column data.

A4 A3 adds a new column of amount, and its value is the sales quantity Quantity multiplied by the product price Price.  

## Associative query

Perform an associated query on the data in the two data tables.

Example: Still use the two files in the previous section to query sales orders with product prices greater than 20 yuan.

|+|A|
|:-|:-|
|1|=T(“e:/orders/sales.xlsx”)|
|2|=T(“e:/orders/product.xlsx”).select(Price>20).keys(ID)|
|3|=A1.switch@i(ProductID,A2)|

A1 Read sales order data.

A2 Read the product information data, select the product information with a price greater than 20, and then set the ID as the main key. 

A3  according to ProductID, A1 to be associated with the primary key of A2, the option @i represents A2 is not found in the ProductID match the product ID, then this record was filtered off.   

## Query main table and subtable

Associative query on the main table and detailed table data, Example: Some data in the employee information table employee.xlsx and employee family member table family.xlsx are as follows. Please query the information of employees who have an elderly person over 70 years old at home.

<img scr="http://img.raqsoft.com.cn/uploads/0319/161613983600008cc.png">

<im scr="http://img.raqsoft.com.cn/uploads/0319/1616139836000586b.png">

|+|A|
|:-|:-|
|1|=T("e:/work/employee.xlsx")|
|2|=T("e:/work/family.xlsx").select(age(Birthday)>=70)|
|3|=join(A1:employee,Eid;A2:family,Eid)|
|4|=A3.conj(employee)|


A1 Read employee data.  

A2 Read employee family member data and select members over 70.  

A3 associates A1 and A2 according to Eid, deletes unmatched records, and names A1 as employee and A2 as family.  

A4 takes out the employee column in A3 and joins it as a table sequence 
