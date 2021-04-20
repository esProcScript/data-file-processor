Excel VBA often fails if there is a huge amount of data. In this case, a professional stand-alone data computing tool will be helpful. esProc is one of its kind, an outstanding one. There will be no need to write complex and inefficient VBA code, but a few lines of SPL code will suffice instead. With esProc at hand, merging Excel worksheets will no longer be a nightmare.

<img src="http://img.raqsoft.com/file/2018/12/61810e052e9c44c1bfd452700afd3000_001.png">

Here are several Excel files in which every worksheet containing a year’s sales data has the same structure as following:

|Customer ID|Customer Name|Invoice Number|Sale Amount|Purchase Date|
|:-|:-|:-|:-|:-|
|1234|John Smith|100-0002|$1,200.00 |2013/1/1|
|2345|Mary Harrison|100-0003|$1,425.00 |2013/1/6|
|3456|Lucy Gomez|100-0004|$1,390.00 |2013/1/11|
|4567|Rupert Jones|100-0005|$1,257.00 |2013/1/18|
|5678|Jenny Walters|100-0006|$1,725.00 |2013/1/24|
|6789|Samantha Donaldson|100-0007|$1,995.00 |2013/1/31|

esProc SPL script:

|　|A|B|
|:-|:-|:-|
|1|for directory@p("d:/excel/\*.xlsx")|=file(A1).xlsopen()|
|2|　|=B1.conj(B1.xlsimport@t('Customer Name','Sale Amount','Purchase Date';\~.stname))|
|3|　|=@|B2|
|4|> file("d:/result.xlsx"). xlsexport@t(B3;"merge_data")|　|


The desired merging effect:

|Customer Name|Sale Amount|Purchase Date|
|:-|:-|:-|
|John Smith|1200|2013/1/1|
|Mary Harrison|1425|2013/1/6|
|Lucy Gomez|1390|2013/1/11|
|Rupert Jones|1257|2013/1/18|
|......|......|......|
|Thomas Haines|1346|2013/2/17|

Explanation:

A1: The for loop traverses every Excel file under the given directory to perform a series of operations over every worksheet in B1~B3.
  
B1: Open an Excel file and generate a sequence.
  
B2: Import the specified fields, ‘Customer Name’, ‘Sale Amount’ and ‘Purchase Date’, from every worksheet of the current file and merge them. This is similar to the operations in A2 in the previous instance.
  
B3: Combine B2’s table sequence with the current value of B3.

A4: Save B3’s table sequence as a worksheet, named merge_data, in file result.xlsx.

The program achieves multiple Excel file merging with only two rounds loop. The outer loop traverses each of the Excel files under the given directory while the inner loop B1.conj() function merges data from every worksheet in an Excel file.

