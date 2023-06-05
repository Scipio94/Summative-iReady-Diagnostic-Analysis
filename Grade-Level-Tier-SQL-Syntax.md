# Grade Level Tier SQL Syntax

**Description**: The following SQL syntax was used to calculate the percentage of students that were in Tiers 1,2,or 3 based on their iReady ELA diagnostic results per grade level. Calculaitons were made for each iReady administration and wrapped in a CTE. All tables in the CTE was combined using the ***UNION ALL*** function

~~~ SQL 
--TIERS Grade
/*Creation for CTE to combine all tables*/
WITH ir1 AS
(/*iReady 1 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir1_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir1_Tier_Level,sub.Student_Grade) AS Tier_Level_Count, 
  CASE -- Creating index for ordering
    WHEN sub.ir1_Tier_Level = 'Tier 1' THEN 1.0
    WHEN sub.ir1_Tier_Level = 'Tier 2' THEN 1.3
    WHEN sub.ir1_Tier_Level = 'Tier 3' THEN 1.6
    END AS iReady_Index, 
  sub.Student_Grade,  
  ROUND(COUNT(*) OVER (PARTITION BY sub.ir1_Tier_Level,sub.Student_Grade) / COUNT(*) OVER (PARTITION BY sub.Student_Grade),2) AS ir1_Percentage,
  COUNT(*) OVER (PARTITION BY sub.Student_Grade) AS Total,
FROM
(SELECT
  CAST(REPLACE(Student_Grade,'K','0')AS numeric) AS Student_Grade, 
  CONCAT(First_Name,' ',Last_Name) AS Name,
  Overall_Relative_Placement,
  CASE -- Creating Tier Level
    WHEN Overall_Relative_Placement IN ('Early On Grade Level','Mid or Above Grade Level') THEN 'Tier 1'
    WHEN Overall_Relative_Placement = '1 Grade Level Below' THEN 'Tier 2'
    WHEN Overall_Relative_Placement IN ('2 Grade Levels Below','3 or More Grade Levels Below') THEN 'Tier 3'
    END AS ir1_Tier_Level,
  CASE -- Creating proficiency CASE
    WHEN Overall_Relative_Placement IN ('Early On Grade Level','Mid or Above Grade Level') THEN 'On Grade Level'
    ELSE 'Not On Grade Level' 
    END AS proficiency,
FROM `my-data-project-36654.iReady_SY_2223.iReady_ELA_1`) AS sub
ORDER BY sub.ir1_Tier_Level),

ir2 AS
(/*iReady 2 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir2_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir2_Tier_Level, sub.Student_Grade) AS ir2_Tier_Level_Count, 
  CASE -- Creating index for ordering
  WHEN sub.ir2_Tier_Level = 'Tier 1' THEN 1.1
  WHEN sub.ir2_Tier_Level = 'Tier 2' THEN 1.4
  WHEN sub.ir2_Tier_Level = 'Tier 3' THEN 1.7
  END AS iReady_Index,
  sub.Student_Grade,
  ROUND(COUNT(*) OVER (PARTITION BY  sub.ir2_Tier_Level,sub.Student_Grade) / COUNT(*) OVER (PARTITION BY sub.Student_Grade),2) AS ir2_Percentage,
  COUNT(*) OVER (PARTITION BY sub.Student_Grade) AS Total,
FROM
(SELECT
  CAST(REPLACE(Student_Grade,'K','0')AS numeric) AS Student_Grade, 
  CONCAT(First_Name,' ',Last_Name) AS Name,
  Overall_Relative_Placement,
  CASE -- Creating Tier Levels
    WHEN Overall_Relative_Placement IN ('Early On Grade Level','Mid or Above Grade Level') THEN 'Tier 1'
    WHEN Overall_Relative_Placement = '1 Grade Level Below' THEN 'Tier 2'
    WHEN Overall_Relative_Placement IN ('2 Grade Levels Below','3 or More Grade Levels Below') THEN 'Tier 3'
    END AS ir2_Tier_Level,
  CASE -- Creating Proficiency CASE
    WHEN Overall_Relative_Placement = 'Mid or Above Grade Level' THEN 'On Grade Level'
    ELSE 'Not On Grade Level' 
    END AS proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_2_ELA`) AS sub
ORDER BY ir2_Tier_Level),

ir3 AS
(/*iReady 3 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir3_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir3_Tier_Level,sub.Student_Grade) AS ir3_Tier_Level_Count, 
  CASE -- Creating index for ordering
    WHEN sub.ir3_Tier_Level = 'Tier 1' THEN 1.2
    WHEN sub.ir3_Tier_Level = 'Tier 2' THEN 1.5
    WHEN sub.ir3_Tier_Level = 'Tier 3' THEN 1.8
    END AS iReady_Index,
  sub.Student_Grade,
  ROUND(COUNT(*) OVER (PARTITION BY  sub.ir3_Tier_Level,sub.Student_Grade) / COUNT(*) OVER (PARTITION BY sub.Student_Grade),2) AS ir3_Percentage,
  COUNT(*) OVER (PARTITION BY sub.Student_Grade) AS Total
FROM
(SELECT
  CAST(REPLACE(Student_Grade,'K','0')AS numeric) AS Student_Grade, 
  CONCAT(First_Name,' ',Last_Name) AS Name,
  Overall_Relative_Placement,
  CASE -- Creating Tier Levels
    WHEN Overall_Relative_Placement IN ('Early On Grade Level','Mid or Above Grade Level') THEN 'Tier 1'
    WHEN Overall_Relative_Placement = '1 Grade Level Below' THEN 'Tier 2'
    WHEN Overall_Relative_Placement IN ('2 Grade Levels Below','3 or More Grade Levels Below') THEN 'Tier 3'
    END AS ir3_Tier_Level,
  CASE -- Creating Profiency CASE
    WHEN Overall_Relative_Placement = 'Mid or Above Grade Level' THEN 'On Grade Level'
    ELSE 'Not On Grade Level' 
    END AS proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_3_ELA`) AS sub
ORDER BY ir3_Tier_Level)

/*Combiing all three tables for calculation*/

SELECT 
  sub1.iReady_Index,--index
  CASE -- Assigning iReady admin based on index
    WHEN sub1.iReady_Index IN (1.0,1.3,1.6) THEN 'iReady 1'
    WHEN sub1.iReady_Index IN (1.1,1.4,1.7) THEN 'iReady 2'
    WHEN sub1.iReady_Index IN (1.2,1.5,1.8) THEN 'iReady 3'
    END iReady_Admin,
  sub1.student_grade,
  sub1.Tier_Level,
  sub1.Tier_Level_Count,
  sub1.Total,
  sub1.Percentage,
FROM 
(SELECT 
  sub.iReady_Index,
  sub.student_grade,
  sub.Tier_Level,
  sub.Tier_Level_Count,
  sub.Total,
  sub.Percentage,
FROM
(
/*Combining all CTEs into single dataset*/
  SELECT
  ir1.iReady_Index, ir1.ir1_Tier_Level AS Tier_Level,ir1.Tier_Level_Count,ir1.Total,ir1_Percentage AS Percentage, ir1.student_grade
FROM ir1 --iReady 1 Data
UNION ALL
SELECT
  ir2.iReady_Index, ir2.ir2_Tier_Level AS Tier_Level,ir2.ir2_Tier_Level_Count,ir2.Total,ir2_Percentage AS Percentage, ir2.student_grade
FROM ir2 -- iReasdy 2 Data
UNION ALL
SELECT
  ir3.iReady_Index, ir3.ir3_Tier_Level AS Tier_Level,ir3.ir3_Tier_Level_Count,ir3.Total,ir3_Percentage AS Percentage, ir3.student_grade
FROM ir3 -- iReady 3 Data
ORDER BY iReady_Index) AS sub -- 1st subquery
ORDER BY sub.iReady_Index) AS sub1 -- 2nd subquery
ORDER BY sub1.student_grade, sub1.iReady_Index; 
~~~
