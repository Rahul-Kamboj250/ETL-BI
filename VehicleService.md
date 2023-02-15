# ETL-BI
SSIS/SQL Server &amp; Power BI 
The Data prep part has been done using Excel, Notepad++ , SSIS/SQL-SERVER.

The data is cleaned using excel; the date format has been changes as required and currency type has been limited to float(2).
Before uploading data into the DataBase; The data types for all columns has been set to text.
In SSIS; the data length for columns is changed to 1000 and 6000 with huge text to avoid truncation error; and data type is DT-STR. The check for Skewed rows both right & left is done by using  Functions AND SKEWED ROWS are routed to flat file using conditional split.
The skewed rows are checked  if those can be repaired by comparing the skewed rows with original file in Notepad++ (Checks done: text qualifiers, decimal places in floats changed as text)
Atlast the additional errors are pointed out in the SQLserver by running detailed analysis and creating a working table in required data types from the RAW text table  by writing the following stored procedure. The data type conversion is relied on implicit data conversion(especially for VehicleService20230213).


USE [VehicleService]
GO
/****** Object:  StoredProcedure [dbo].[BLD_WRK_VehicleService20230213]    Script Date: 2/14/2023 3:41:06 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		Rahul Kamboj
-- Create date: 20230213
-- Description:	RAW TO WRK VehicleService Table
-- =============================================
ALTER PROC [dbo].[BLD_WRK_VehicleService20230213]


AS BEGIN
-- ================================================
--DROP TABLE
-- ================================================
IF OBJECT_ID('WRK_Vehicle_Service20230213') is not NULL
DROP TABLE[WRK_Vehicle_Service20230213]
-- ================================================
--CREATE TABLE
-- ================================================
CREATE TABLE[WRK_Vehicle_Service20230213](
       [Row Number]			INT IDENTITY(1,1)
      ,[CustomerID]			VARCHAR(100)
      ,[CustomerSince]		DATE
      ,[Vehicle]			VARCHAR(100)
      ,[2014]				FLOAT
      ,[2015]				FLOAT
      ,[2016E]				FLOAT
)
-- ================================================
--TRUNCATE TABLE
-- ================================================
TRUNCATE TABLE[WRK_Vehicle_Service20230213]

-- ================================================
--INSERT INTO
-- ================================================
INSERT INTO[WRK_Vehicle_Service20230213](
      [CustomerID]
      ,[CustomerSince]
      ,[Vehicle]
      ,[2014]
      ,[2015]
      ,[2016E]

)
SELECT 
	   [CustomerID]
      ,[CustomerSince]
      ,[Vehicle]
      ,[2014]
      ,[2015]
      ,[2016E]

FROM [VehicleService].[dbo].[VehicleService20230213]
--EXCLUSIONS
where ISNUMERIC([2015]) <> 0  --(1  ROW excluded)

--(1049998 rows affected)

--excluded row
  /*select * from [VehicleService].[dbo].[VehicleService20230213]
  where ISNUMERIC([2015]) <> 1 and  [2015] <> '' */

 /* --Additional checks for anomalies

  --Check for duplicates
    SELECT * FROM  [VehicleService].[dbo].[WRK_Vehicle_Service20230213]
	WHERE [CustomerID] LIKE '3490750'

  --Check for [CustomerSince] Column

  SELECT * FROM [VehicleService].[dbo].[WRK_Vehicle_Service20230213]
  WHERE [CustomerSince] < '1999-01-01'
*/
END
