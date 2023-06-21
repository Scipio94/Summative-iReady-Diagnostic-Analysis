# Network Proficiency SQL Syntax

**Description** : The following SQL syntax was used to calculate the percentage of students that were proficient, on grade level, based on their iReady ELA diagnostic results. Calculaitons were made for each iReady administration and wrapped in a CTE. All tables in the CTE was combined using the ***UNION ALL*** function.

~~~ SQL 
--On Grade Level Overall
/*Creation for CTE to combine all tables*/
WITH ir1 AS
(/*iReady 1 ELA Metrics*/ 
SELECT 
  DISTINCT sub.Proficiency, 
  COUNT(*) OVER (PARTITION BY sub.Proficiency) AS Proficiency_Count, 
  CASE -- Creating index for ordering
  WHEN sub.Proficiency = 'On Grade Level' THEN 1.00
  WHEN sub.Proficiency = 'Not On Grade Level' THEN 1.04
  END AS iReady_Index, 
  ROUND(COUNT(*) OVER (PARTITION BY sub.Proficiency) / COUNT(*) OVER (),2) AS ir1_Proficiency_Percentage,
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
    END AS Proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_ELA_1`) AS sub),

ir2 AS
(/*iReady 2 ELA Metrics*/ 
SELECT 
  DISTINCT sub.Proficiency, 
 COUNT(*) OVER (PARTITION BY sub.Proficiency) AS Proficiency_Count, 
  CASE -- Creating index for ordering
  WHEN sub.Proficiency = 'On Grade Level' THEN 1.01
  WHEN sub.Proficiency = 'Not On Grade Level' THEN 1.05
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY sub.Proficiency) / COUNT(*) OVER (),2) AS ir2_Proficiency_Percentage,
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
FROM `my-data-project-36654.iReady_SY_2223.iReady_2_ELA`) AS sub),

ir3 AS
(/*iReady 3 ELA Metrics*/ 
SELECT 
  DISTINCT sub.Proficiency, 
  COUNT(*) OVER (PARTITION BY sub.Proficiency) AS Proficiency_Count, 
  CASE -- Creating index for ordering
  WHEN sub.Proficiency = 'On Grade Level' THEN 1.02
  WHEN sub.Proficiency = 'Not On Grade Level' THEN 1.06
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY sub.Proficiency) / COUNT(*) OVER (),2) AS ir3_Proficiency_Percentage,
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
    END AS Proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_3_ELA`) AS sub),

ir4 AS
(/*iReady 4 ELA Metrics*/ 
SELECT 
  DISTINCT sub.Proficiency, 
  COUNT(*) OVER (PARTITION BY sub.Proficiency) AS Proficiency_Count, 
  CASE -- Creating index for ordering
  WHEN sub.Proficiency = 'On Grade Level' THEN 1.03
  WHEN sub.Proficiency = 'Not On Grade Level' THEN 1.07
  END AS iReady_Index,
  ROUND(COUNT(*) OVER (PARTITION BY sub.Proficiency) / COUNT(*) OVER (),2) AS ir4_Proficiency_Percentage,
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
    END AS Proficiency
FROM `my-data-project-36654.iReady_SY_2223.iReady_4_ELA`) AS sub)



/*Combiing all tables for calculation*/

SELECT 
  sub1.iReady_Index,--index
  CASE -- Assigning iReady admin based on index
    WHEN sub1.iReady_Index IN (1.00,1.04) THEN 'iReady 1'
    WHEN sub1.iReady_Index IN (1.01,1.05) THEN 'iReady 2'
    WHEN sub1.iReady_Index IN (1.02,1.06) THEN 'iReady 3'
    WHEN sub1.iReady_Index IN (1.03,1.07) THEN 'iReady 4'
    END iReady_Admin,
  sub1.Proficiency,
  sub1.Proficiency_Count,
  sub1.Total,
  sub1.Percentage
FROM 
(SELECT 
  sub.iReady_Index,
  sub.Proficiency,
  sub.Proficiency_Count,
  sub.Total,
  sub.Percentage,
FROM
(
/*Combining all CTEs into single dataset*/
  SELECT
  ir1.iReady_Index,ir1.Proficiency,ir1.Proficiency_Count,ir1.ir1_Proficiency_Percentage AS Percentage,Total
FROM ir1 --iReady 1 Data
UNION ALL
SELECT
  ir2.iReady_Index,ir2.Proficiency,ir2.Proficiency_Count,ir2.ir2_Proficiency_Percentage AS Percentage, Total
FROM ir2 -- iReasdy 2 Data
UNION ALL
SELECT
  ir3.iReady_Index,ir3.Proficiency,ir3.Proficiency_Count,ir3.ir3_Proficiency_Percentage AS Percentage,Total
FROM ir3 -- iReady 3 Data
UNION ALL
SELECT
  ir4.iReady_Index,ir4.Proficiency,ir4.Proficiency_Count,ir4.ir4_Proficiency_Percentage AS Percentage,Total
FROM ir4 -- iReady 4 Data
ORDER BY iReady_Index) AS sub -- 1st subquery
ORDER BY sub.iReady_Index) AS sub1 -- 2nd subquery
ORDER BY sub1.iReady_Index; 
~~~
