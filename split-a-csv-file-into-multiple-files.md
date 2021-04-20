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


A1 Create the cursor to import the original Excel files.

A2 Loop through the cursor to retrieve 50,000 rows each time (the number is determined according to the memory space).

B2 Group each batch of retrieved rows by partName.

B3 Loop through each group of parts.

C3 Create a file object named after the parts name.

C4-D5 If a file object already exists, use @a option to append the parts orders to it; if there isn’t such a file object, use @t to import column headers first and then the detailed data.

2.  Split of multi-row records
In some text files, a record consists of multiple rows. To split such a file, we need to identify the rows for forming one record and make sure that one record won’t be split into two files.

Example 1: log.txt records web operations logs. Each piece of log occupies 5 lines. The task is to split the file into smaller files, and each will contain 1000 pieces of logs.

esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file(“e:/log.txt”:”UTF-8”).cursor@s()|　|
|2|for A1,5000|=file(“e:/log/log_”/#A2/”.txt”).export(A2)|

A1 Import the log file with the cursor; @s option enables reading the whole line as a string.

A2 Loop through the cursor to retrieve 5000 rows each time, which forms 1000 piece of logs.

B2 Generate logs file names according to the loop number, such as log_1.txt, log_2.txt… and export all retrieved rows in the current time to A2.

 

Example 2: log.txt stores application execution logs, as shown below. Each piece of log is made up of an indefinite number of lines that starts with a left square bracket and ends before another left square bracket. The task is to split the file into multiple files and each will contain 1000 logs.

```
[2018-05-14 09:20:20]
ERROR: NullPointer exception in line 27
[2018-05-14 09:20:20]
DEBUG: Temporary file directory is:
D: temp esProc nodes127.0.0.1_ 8281 temp.
Files in temporary directory will be deleted on every 12 hours.
```
esProc SPL script:

|+|A|B|
|:-|:-|:-|
|1|=file(“e:/log.txt”:”UTF-8”).read@n()|=A1.select(\~!="")|
|2|=B1.group@i(pos(\~,"\[")==1).cursor()|　|
|3|for A2,1000|=file(“e:/log/log_”/#A3/”.txt”)|
|4|　|=A3.(\~.(B3.write@a(\~)))|

A1 Open the logs file to read in data; @n option enables reading each line as a string and returns a sequence of strings.

B1 Get non-empty rows.

A2 Group these rows according to whether a row starts with a left square bracket. Create a new group if a row begins with a left square bracket and put a row into the current group if it doesn’t. Then read in all groups of sequences as a cursor.

A3 Loop through the cursor to retrieve 1000 groups each time, that is, 1000 pieces of logs.

B3 Generate logs file names according to the loop number, such as log_1.txt, log_2.txt….

B4 Two levels of loops. The outer loop is 1000 groups and the inner loop is the rows in each group. Write each row to the end of the target file.





