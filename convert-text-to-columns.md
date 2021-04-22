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

