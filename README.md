# examples-for-mongodb-with-esproc
|　|A|B|C|
|:-|:-|:-|:-|
|1|=create(Customer_Code,Customer_Name,
Quotation_No,Unit_Price,Contract_Expiry_Date)|
|2|=file("E:/txt2excel/item.txt").read@n()|　|　|
|3|for   A2|if   len(A3)<136|next|
|4|　|=right(left(A3,136),-58)|=B4.split@tp()|
|5|　|if   !ifnumber(C4(1))|next|
|6|　|=C4.m(4:C4.len()-1).concat(" ")|　|
|7|　|>A1.insert(0,C4(3),B6,C4(2),C4(1),C4(C4.len()))|　|
|8|=file("E:/txt2excel/item.xlsx").xlsexport@t(A1)|　|　|

^Table ^formatting ^brought ^to ^you ^by ^[ExcelToReddit](https://xl2reddit.github.io/)

