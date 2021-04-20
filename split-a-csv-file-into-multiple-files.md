# Split a CSV file into multiple files

## Split by grouping

To split a file, we divide data in the file into a number of groups, write each group into a new file and name the new file after the group name.

Example: An Excel file records orders of various parts. We want to split it and store orders of each type of parts into a new Excel file to send them to the parts manufacturer.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file(“e:/orders/orders.xlsx”).xlsimport@t()|=A1.group(partName)|
|2|for B1|=file("e:/parts/"+A2(1).partName+”.xlsx”).xlsexport@t(A2)|

A1 Import all original Excel files.

B1 Group imported data by partName.

A2 Loop through each group of parts orders.

B2 Export orders of each type of parts to a new Excel file named after the parts name.


If the size of the original files is too big to be loaded into the memory at a time, we can import it with the cursor using the following script:

|+|A|B|C|D|
|:-|:-|:-|:-|:-|
|1|=file(“e:/orders/orders.xlsx”)<br>.xlsimport@tc()|　|　|　|
|2|for A1,50000|=A2.group(partName)|　|　|
|3|　|for B2|=file("e:/parts/"+B3(1).partName+”.xlsx”)|　|
|4|　|　|if C3.exists()|=C3.xlsexport@a(B3)|
|5|　|　|else|=C3.xlsexport@t(B3)|

^Table ^formatting ^brought ^to ^you ^by ^[ExcelToReddit](https://xl2reddit.github.io/)


A1 Create the cursor to import the original Excel files.

A2 Loop through the cursor to retrieve 50,000 rows each time (the number is determined according to the memory space).

B2 Group each batch of retrieved rows by partName.

B3 Loop through each group of parts.

C3 Create a file object named after the parts name.

C4-D5 If a file object already exists, use @a option to append the parts orders to it; if there isn’t such a file object, use @t to import column headers first and then the detailed data.









