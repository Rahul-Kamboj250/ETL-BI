# ETL-BI
# The Data prep part has been done using Excel, Notepad++ , SSIS/SQL-SERVER.

The data is cleaned using excel; the date format has been changes as required and currency type has been limited to float(2).
Before uploading data into the DataBase; The data types for all columns has been set to text.
In SSIS; the data length for columns is changed to 1000 and 6000 with huge text to avoid truncation error; and data type is DT-STR. The check for Skewed rows both right & left is done by using  Functions AND SKEWED ROWS are routed to flat file using conditional split.
The skewed rows are checked  if those can be repaired by comparing the skewed rows with original file in Notepad++ (Checks done: text qualifiers, decimal places in floats changed as text)
Atlast the additional errors are pointed out in the SQLserver by running detailed analysis and creating a working table in required data types from the RAW text table  by writing the following stored procedure. The data type conversion is relied on implicit data conversion(especially for VehicleService20230213).

