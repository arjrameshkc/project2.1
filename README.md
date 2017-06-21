# project2.1


 1.
REGISTER /usr/local/pig/lib/piggybank.jar;
DEFINE XPath org.apache.pig.piggybank.evaluation.xml.XPath();
row =  LOAD 'flume_import2' using org.apache.pig.piggybank.storage.XMLLoader('row') as (x:chararray);
data = FOREACH row GENERATE XPath(x, 'row/State_Name') AS State_Name, 
    XPath(x, 'row/District_Name') AS District_Name,
    XPath(x, 'row/Project_Objectives_IHHL_BPL') AS IHHL_BPL,  
    XPath(x, 'row/Project_Objectives_IHHL_APL') AS IHHL_APL,  
    XPath(x, 'row/Project_Objectives_IHHL_TOTAL') AS IHHL_TOTAL,  
    XPath(x, 'row/Project_Objectives_SCW') AS SCW,
    XPath(x, 'row/Project_Objectives_School_Toilets') AS SToilets,
    XPath(x, 'row/Project_Objectives_Anganwadi_Toilets') AS AToilets,
    XPath(x, 'row/Project_Objectives_RSM') AS RSM,
    XPath(x, 'row/Project_Objectives_PC') AS PC,
    XPath(x, 'row/Project_Performance-IHHL_BPL') AS PIHHL_BPL,
    XPath(x, 'row/Project_Performance-IHHL_APL') AS PIHHL_APL,
    XPath(x, 'row/Project_Performance-IHHL_TOTAL') AS PIHHL_TOTAL,
    XPath(x, 'row/Project_Performance-SCW') AS PSCW,
    XPath(x, 'row/Project_Performance-School_Toilets') AS PSToilets,
    XPath(x, 'row/Project_Performance-Anganwadi_Toilets') AS PAToilets,
    XPath(x, 'row/Project_Performance-RSM') AS PRSM,
    XPath(x, 'row/Project_Performance-PC') AS PPC;
Filter_data = filter data by PIHHL_BPL >= IHHL_BPL;
District_name = FOREACH Filter_data GENERATE District_Name;
Store District_name into '/project2/pig/District_name';

MySQL -u root -p

Show tables;
Desc District;
/project2/pig/District_name/part-m-00000 - hdfs file stored location

sqoop export -m 1 --connect jbdc:MYSQL://localhost/test --username acadgild --table District --export-dir /project2/pig/District_name/

2.

REGISTER /usr/local/pig/lib/piggybank.jar;
DEFINE XPath org.apache.pig.piggybank.evaluation.xml.XPath();
row =  LOAD 'flume_import2' using org.apache.pig.piggybank.storage.XMLLoader('row') as (x:chararray);
data = FOREACH row GENERATE XPath(x, 'row/State_Name') AS State_Name, 
    XPath(x, 'row/District_Name') AS District_Name,
    XPath(x, 'row/Project_Objectives_IHHL_BPL') AS IHHL_BPL,  
    XPath(x, 'row/Project_Objectives_IHHL_APL') AS IHHL_APL,  
    XPath(x, 'row/Project_Objectives_IHHL_TOTAL') AS IHHL_TOTAL,  
    XPath(x, 'row/Project_Objectives_SCW') AS SCW,
    XPath(x, 'row/Project_Objectives_School_Toilets') AS SToilets,
    XPath(x, 'row/Project_Objectives_Anganwadi_Toilets') AS AToilets,
    XPath(x, 'row/Project_Objectives_RSM') AS RSM,
    XPath(x, 'row/Project_Objectives_PC') AS PC,
    XPath(x, 'row/Project_Performance-IHHL_BPL') AS PIHHL_BPL,
    XPath(x, 'row/Project_Performance-IHHL_APL') AS PIHHL_APL,
    XPath(x, 'row/Project_Performance-IHHL_TOTAL') AS PIHHL_TOTAL,
    XPath(x, 'row/Project_Performance-SCW') AS PSCW,
    XPath(x, 'row/Project_Performance-School_Toilets') AS PSToilets,
    XPath(x, 'row/Project_Performance-Anganwadi_Toilets') AS PAToilets,
    XPath(x, 'row/Project_Performance-RSM') AS PRSM,
    XPath(x, 'row/Project_Performance-PC') AS PPC;
Filter_data80 = filter data by PIHHL_BPL >= 0.8*IHHL_BPL;
District_name80 = FOREACH Filter_data80 GENERATE District_Name;
Store District_name80 into '/project2/pig/District_name80';

sqoop export -m 1 --connect jbdc:MYSQL://localhost/test --username acadgild --table District80 --export-dir /project2/pig/District_name80/
