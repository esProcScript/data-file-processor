# Convert Text to Columns with expressions

Though Excel functions are good at handling simple string split tasks, they become awkward for handling certain special string splits and those having complicated conditions. This article gives examples of the difficult tasks, explains the challenges and offers solutions in esProc SPL. It’s convenient to handle structured data processing, including string split of course, with esProc.

SPL can read in an Excel file directly. To do a real-time analysis, it copies the Excel data onto the clipboard, uses the clipboard function to get the data, writes the processed data back to the clipboard and then pastes the result set to Excel. The whole process is smooth, seamless, convenient and efficient.


## Split by words

We have a product purchase list. In the list, each item includes the brand and type we want. What we want is to split each item to respectively list the brand and type after it.

Below is productlist.xlsx:

<img src="http://img.raqsoft.com.cn/uploads/0917/160031273600041dd.png">

The expected result:

<img src="http://img.raqsoft.com.cn/uploads/0917/1600312748000e416.png">

esProc SPL script:

| |A|B|
|:-|:-|:-|
|1|=clipboard().import@i()|/ Import the product list   directly form the clipboard|
|2|=A1.(\~.split@1(""))|/ Split each item of the   product list by the first space to generate a sequence of sequences|
|3|=A2.concat@n("\t")|/ Concatenate A2 into a   string of values of two-dimensional table, where members in each subsequence   are delimited by tab and member sequences are separated by carriage return |
|4|=clipboard(A3)|/ Put the string of values   onto the clipboard|

After the SPL script is executed, we just need to paste the result set onto B1 of the Excel file to get the desired result.

## Split away digits

There are strings made up of both digits and characters. We want to separate the digitss from the characters.

Below is numbers.xlsx:

<img src="http://img.raqsoft.com.cn/uploads/0917/160031276000001be.png">

The expected result:

<img src="http://img.raqsoft.com.cn/uploads/0917/16003127700006ee0.png">

SPL can directly split a string into individual characters and categorize them by type:

| |A|B|
|:-|:-|:-|
|1|=clipboard().split@n()|/Get data from the   clipboard, split it into a sequence by carriage return and then split each   member character by character|
|2|=A1.(\~.align@a(\[true,false\],isdigit(\~)).(\~.concat()))|/Group each sequence of   characters according to whether a character is a digit or not and concatenate   each group into a string to split digits from the characters|
|3|=A2.concat@n("\t")|/Concatenate the two levels   of sequences into a two-dimensional table string by tab and carriage return   respectively|
|4|=clipboard(A3)|/ Paste A3’s string onto the   clipboard|

## Split away dates

Here are rows of words containing dates. We want to split away all dates from each row and separate the dates by semicolon there are more than one date.

Below is multidates.xlsx:

<img src="http://img.raqsoft.com.cn/uploads/0917/1600312790000fa28.png">

The expected result:

<img src="http://img.raqsoft.com.cn/uploads/0917/16003127990007ed2.png">

SPL’s way is to split a string into a sequence of words by spaces and convert members according to the specified date format:

| |A|B|
|:-|:-|:-|
|1|=clipboard().split@n(“ “)|/ Get data from the   clipboard, split it into a sequence by carriage return and then split each   member into a sequence of words|
|2|=A1.(\~.   (date(\~,"dd.MM.yy")))|/ Convert members in the   sequence of words into date type data according to the specified format|
|3|=A2.(\~.select(ifdate(\~)))|/ Get date type values from   the sequence|
|4|=clipboard(A3.concat@n(“;”))|/ Concatenate the date   string into a two-dimensional data string and paste it onto the clipboard|

After the SPL script is executed, we just need to paste the result set onto B1 of the Excel file to get the desired result.

## Split by character

The following table contains a column of numbers of different lengths. We want to split each digit away from each numeric value to become an individual column.

Below is number.xlsx:

<img src="http://img.raqsoft.com.cn/uploads/0917/16003128210006bef.png">

The expected result:

<img src="http://img.raqsoft.com.cn/uploads/0917/16003128300006670.png">

SPL can do the splitting by characters directly:

| |A|B|
|:-|:-|:-|
|1|=clipboard().split@n()|/ Get data from the   clipboard, split it into a sequence by carriage return and then, by default,   split each member into a sequence of individual characters|
|2|=A1.concat@n("\t")|/ Concatenate the two levels   of sequences into a two-dimensional table string|
|3|=clipboard(A2)|/ Paste the result set onto the clipboard|

After the SPL script is executed, we just need to paste the result set onto B1 of the Excel file to get the desired result.

## Split away attribute tables & file names

Here is a log file of complicated structure. It contains description of nodes that are similar to an attribute table. Now we want to split away the values of PublicKeyToken and the file names from the attribute description.

Below is log.xlsx:

<img src="http://img.raqsoft.com.cn/uploads/0917/160031284100045a9.png">

The expected result:

<img src="http://img.raqsoft.com.cn/uploads/0917/160031285000056ac.png">

SPL has the special function to directly get values of attribute strings and get different parts of a file name:

| |A|B|
|:-|:-|:-|
|1|=clipboard().split@nc()|/ Get data from the clipboard, split it into a sequence by carriage return and then, by default, split each member into a sequence by commas|
|2|=A1.(\[replace(\~(2),"\"","").property("PublicKeyToken"),filename(replace(\~(3),"\"",""))\])|/ Remove quotes at both ends, use property function to get PublicKeyToken value from item 2 and   filename function to get the file name from the third item; then join the two values into a sequence|
|3|=clipboard(A2.concat@n("\t"))|/ Paste the result set onto the clipboard|


For detail explanation, see [Sample Programs of String Split in Excel](http://c.raqsoft.com/article/1600314086044)
