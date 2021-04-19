Excel VBA often fails if there is a huge amount of data. In this case, a professional stand-alone data computing tool will be helpful. esProc is one of its kind, an outstanding one. There will be no need to write complex and inefficient VBA code, but a few lines of SPL code will suffice instead. With esProc at hand, merging Excel worksheets will no longer be a nightmare.

Here are several Excel files in which every worksheet containing a year’s sales data has the same structure as the worksheets in the previous instance:

[](http://img.raqsoft.com/file/2018/12/61810e052e9c44c1bfd452700afd3000_001.png)

|　|A|B|
|:-|:-|:-|
|1|for directory@p("d:/excel/\*.xlsx")|=file(A1).xlsopen()|
|2|　|=B1.conj(B1.xlsimport@t('Customer Name','Sale Amount','Purchase Date';\~.stname))|
|3|　|=@|B2|
|4|> file("d:/result.xlsx"). xlsexport@t(B3;"merge_data")|　|

|Customer Name|Sale Amount|Purchase Date|
|:-|:-|:-|
|John Smith|1200|2013/1/1|
|Mary Harrison|1425|2013/1/6|
|Lucy Gomez|1390|2013/1/11|
|Rupert Jones|1257|2013/1/18|
|......|......|......|
|Thomas Haines|1346|2013/2/17|
