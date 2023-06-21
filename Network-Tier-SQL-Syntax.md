# Network Tier SQL Syntax

**Description** : The following SQL syntax was used to calculate the percentage of students that were in Tiers 1,2,or 3 based on their iReady ELA diagnostic results. Calculaitons were made for each iReady administration and wrapped in a CTE. All tables in the CTE was combined using the ***UNION ALL*** function

~~~ SQL
--TIERS
/*Creation for CTE to combine all tables*/
WITH ir1 AS
(/*iReady 1 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir1_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir1_Tier_Level) AS Tier_Level_Count, 
  CASE -- Creating index for ordering
  WHEN sub.ir1_Tier_Level = 'Tier 1' THEN 1.00
  WHEN sub.ir1_Tier_Level = 'Tier 2' THEN 1.04
  WHEN sub.ir1_Tier_Level = 'Tier 3' THEN 1.08
  END AS iReady_Index, 
  ROUND(COUNT(*) OVER (PARTITION BY sub.ir1_Tier_Level) / COUNT(*) OVER (),2) AS ir1_Percentage,
  COUNT(*) OVER () AS Total
FROM
(SELECT
  Student_Grade, 
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
    END AS proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_ELA_1`) AS sub
ORDER BY sub.ir1_Tier_Level),

ir2 AS
(/*iReady 2 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir2_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir2_Tier_Level) AS ir2_Tier_Level_Count, 
  CASE -- Creating index for ordering
  WHEN sub.ir2_Tier_Level = 'Tier 1' THEN 1.01
  WHEN sub.ir2_Tier_Level = 'Tier 2' THEN 1.05
  WHEN sub.ir2_Tier_Level = 'Tier 3' THEN 1.09
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY  sub.ir2_Tier_Level) / COUNT(*) OVER (),2) AS ir2_Percentage,
  COUNT(*) OVER () AS Total
FROM
(SELECT
  Student_Grade, 
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
  COUNT(*) OVER (PARTITION BY sub.ir3_Tier_Level) AS ir3_Tier_Level_Count, 
  CASE -- Creating index for ordering
  WHEN sub.ir3_Tier_Level = 'Tier 1' THEN 1.02
  WHEN sub.ir3_Tier_Level = 'Tier 2' THEN 1.06
  WHEN sub.ir3_Tier_Level = 'Tier 3' THEN 1.10
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY  sub.ir3_Tier_Level) / COUNT(*) OVER (),2) AS ir3_Percentage,
  COUNT(*) OVER () AS Total
FROM
(SELECT
  Student_Grade, 
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
ORDER BY ir3_Tier_Level), 

ir4 AS 
(/*iReady 4 ELA Metrics*/ 
SELECT 
  DISTINCT sub.ir4_Tier_Level, 
  COUNT(*) OVER (PARTITION BY sub.ir4_Tier_Level) AS ir4_Tier_Level_Count, 
  CASE -- Creating index for ordering
  WHEN sub.ir4_Tier_Level = 'Tier 1' THEN 1.03
  WHEN sub.ir4_Tier_Level = 'Tier 2' THEN 1.07
  WHEN sub.ir4_Tier_Level = 'Tier 3' THEN 1.11
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY  sub.ir4_Tier_Level) / COUNT(*) OVER (),2) AS ir4_Percentage,
  COUNT(*) OVER () AS Total
FROM
(SELECT
  Student_Grade, 
  CONCAT(First_Name,' ',Last_Name) AS Name,
  Overall_Relative_Placement,
  CASE -- Creating Tier Levels
    WHEN Overall_Relative_Placement IN ('Early On Grade Level','Mid or Above Grade Level') THEN 'Tier 1'
    WHEN Overall_Relative_Placement = '1 Grade Level Below' THEN 'Tier 2'
    WHEN Overall_Relative_Placement IN ('2 Grade Levels Below','3 or More Grade Levels Below') THEN 'Tier 3'
    END AS ir4_Tier_Level,
  CASE -- Creating Profiency CASE
    WHEN Overall_Relative_Placement = 'Mid or Above Grade Level' THEN 'On Grade Level'
    ELSE 'Not On Grade Level' 
    END AS proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_4_ELA`) AS sub
ORDER BY ir4_Tier_Level)

/*Combiing all three tables for calculation*/

SELECT 
  sub1.iReady_Index,--index
  CASE -- Assigning iReady admin based on index
    WHEN sub1.iReady_Index IN (1.00,1.04,1.08) THEN 'iReady 1'
    WHEN sub1.iReady_Index IN (1.01,1.05,1.09) THEN 'iReady 2'
    WHEN sub1.iReady_Index IN (1.02,1.06,1.10) THEN 'iReady 3'
    WHEN sub1.iReady_Index IN (1.03,1.07,1.11) THEN 'iReady 4'
    END iReady_Admin,
  sub1.Tier_Level,
  sub1.Tier_Level_Count,
  sub1.Total,
  sub1.Percentage
FROM 
(SELECT 
  sub.iReady_Index,
  sub.Tier_Level,
  sub.Tier_Level_Count,
  sub.Total,
  sub.Percentage,
FROM
(
/*Combining all CTEs into single dataset*/
  SELECT
  ir1.iReady_Index, ir1.ir1_Tier_Level AS Tier_Level,ir1.Tier_Level_Count,ir1.Total,ir1_Percentage AS Percentage
FROM ir1 --iReady 1 Data
UNION ALL
SELECT
  ir2.iReady_Index, ir2.ir2_Tier_Level AS Tier_Level,ir2.ir2_Tier_Level_Count,ir2.Total,ir2_Percentage AS Percentage
FROM ir2 -- iReady 2 Data
UNION ALL
SELECT
  ir3.iReady_Index, ir3.ir3_Tier_Level AS Tier_Level,ir3.ir3_Tier_Level_Count,ir3.Total,ir3_Percentage AS Percentage
FROM ir3 -- iReady 3 Data
UNION ALL
SELECT
  ir4.iReady_Index, ir4.ir4_Tier_Level AS Tier_Level,ir4.ir4_Tier_Level_Count,ir4.Total,ir4_Percentage AS Percentage
FROM ir4 -- iReady 4 Data
ORDER BY iReady_Index) AS sub -- 1st subquery
ORDER BY sub.iReady_Index) AS sub1 -- 2nd subquery
ORDER BY sub1.iReady_Index; 
~~~
