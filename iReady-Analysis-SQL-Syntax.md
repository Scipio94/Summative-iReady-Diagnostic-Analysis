~~~ SQL 
--NETWORK ANALYSIS TIERS
/*Network Tier 1 Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change, 
FROM
(SELECT
  iReady_Index, 
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 1') AS sub
ORDER BY sub.iReady_Index;
--Steady increase of students moving to Tier 1 average of +7% per iReady Admin in ELA

/*Network Tier 1 Range*/
SELECT 
  MAX(Percentage) - MIN(Percentage) AS Network_Tier_1_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 1';

/*Network Tier 2 Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 2') AS sub
ORDER BY sub.iReady_Index;
--There has been a slight decline in the number of students in Tier 2 with a -4% decrease from iReady 2 to iReady 3 and an average change of -1% over each administration.

/*Network Tier 2 Range*/
SELECT 
  ROUND(MAX(Percentage) - MIN(Percentage),2) AS Network_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 2';

/*Network Tier 3 Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 3') AS sub
ORDER BY sub.iReady_Index;
--Steady decline in students in Tier 3, largest change was from iReady 1 to iReady 2, -12%. Average -6% over each administration 

/*Network Tier 3 Range*/
SELECT 
  ROUND(MAX(Percentage) - MIN(Percentage),2) AS Network_Tier_3_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Tier_Analysis`
WHERE Tier_Level = 'Tier 3';

--CAMPUS ANALYSIS TIERS
/*ES Tier 1 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 1') AS sub
ORDER BY sub.iReady_Index;
-- Steady increase in students on Tier 1 in ES. Largest just from iReady 1 to iReady to with an increase of +23%. Average iReady Administration Change +12%

/*ES Campus Tier 1 Range*/
SELECT
  MAX(Percentage) - MIN(percentage) AS ES_Tier_1_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 1';

/*ES Tier 2 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 2') AS sub
ORDER BY sub.iReady_Index;
-- Steady decrease in students on Tier 2 with an average of -6% per iReady Administration

/*ES Campus Tier 2 Range*/
SELECT
  ROUND(MAX(Percentage) - MIN(percentage),2) AS ES_Tier_1_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 2';

/*ES Tier 3 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 3') AS sub
ORDER BY sub.iReady_Index;
-- Steady decline in studnets in Tier 3, largest change was from iReady 1 to iReady 2 -14%. Average change of -7% per iReady administration

/*ES Campus Tier 3 Range*/
SELECT
  MAX(Percentage) - MIN(percentage) AS ES_Tier_1_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'ES' AND Tier_Level = 'Tier 3';

/*MS Tier 1 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 1') AS sub
ORDER BY sub.iReady_Index;
-- Steady increase of students in Tier 1, largest increase from iReady 1 to iReady 2, +7%, average increase of +4% per iReady administration
-- MS had high turnover rates

/*MS Campus Tier 1 Range*/
SELECT
  MAX(Percentage) - MIN(percentage) AS MS_Tier_1_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 1';

/*MS Tier 2 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 2') AS sub
ORDER BY sub.iReady_Index;
-- Increase in students in Tier 2 from iReady 1 to iReady 2, +12%, and decrease from iReady 2 to iReady 3, -5% and overall change of +2% per administration

/*MS Campus Tier 2 Range*/
SELECT
  MAX(Percentage) - MIN(percentage) AS MS_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 2';

/*MS Tier 3 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 3') AS sub
ORDER BY sub.iReady_Index;
-- Decrease in students in Tier 3,largest decrease from iReady 1 to iReady 2, -19%. Average decrease per admin -6%

/*MS Campus Tier 3 Range*/
SELECT
  MAX(Percentage) - MIN(percentage) AS MS_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'MS' AND Tier_Level = 'Tier 3';

/*HS Tier 1 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 1') AS sub
ORDER BY sub.iReady_Index;
-- Slight increase in students in Tier 1, largest increase from iReady 2 to iReady 3, +5%, average increase of +1% per administration 

/*HS Campus Tier 1 Range*/
SELECT
  ROUND(MAX(Percentage) - MIN(percentage),2) AS HS_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 1';

/*HS Tier 2 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 2') AS sub
ORDER BY sub.iReady_Index;
-- Slight increase of students in Tier 2, largest increase fron iReady 1 to iReady 2, +4%. Average increase of +2% per administration

/*HS Campus Tier 2 Range*/
SELECT
  ROUND(MAX(Percentage) - MIN(percentage),2) AS HS_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 2';

/*HS Tier 3 Analysis*/
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Tier_Level,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT 
  iReady_Index, 
  Campus,
  iReady_Admin,
  Tier_Level,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 3') AS sub
ORDER BY sub.iReady_Index;
-- Steady decrease in students in Tier, largest decrease from iReady 2 to iReady 3, -6%. Average decrease -3% per admin, 9% decrease overall

/*HS Campus Tier 3 Range*/
SELECT
  ROUND(MAX(Percentage) - MIN(percentage),2) AS HS_Tier_2_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Tier_Analysis`
WHERE Campus = 'HS' AND Tier_Level = 'Tier 3';

--NETWORK PROFICIENCY ANALYSIS
/*Network Proficiency Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.iReady_Admin,
  sub.Proficiency,
  sub.Percentage,
  ROUND(AVG(sub.Percentage) OVER (),2) AS Overall_Network_AVG, 
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  iReady_Admin,
  Proficiency,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Proficiency_Analysis`
WHERE Proficiency = 'On Grade Level') AS sub
ORDER BY sub.iReady_Index;
-- No steady trend. There was decrease in proficiency from iReady 1 to iReady 2, -6%, and an increase in proficiency from iReady 2 to iReady 3, +8%
-- Why was there a decrease from iReady 1 to iReady 2

/*Network Proficiency Range*/
SELECT
  ROUND(MAX(percentage) - MIN(Percentage),2) AS Network_Proficiency_Range
FROM `my-data-project-36654.iReady_Summative_Analysis.Network_Proficiency_Analysis`
WHERE Proficiency = 'On Grade Level';

/* ES Campus Proficiency Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Proficiency,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  Campus,
  iReady_Admin,
  Proficiency,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Proficiency_Anaysis`
WHERE Proficiency = 'On Grade Level' AND Campus = 'ES') AS sub
ORDER BY sub.iReady_Index;
-- Steady Increase, largest increase iReady 2 to iReady 3, +13%, average of +4% change over each administration

/* MS Campus Proficiency Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Proficiency,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  Campus,
  iReady_Admin,
  Proficiency,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Proficiency_Anaysis`
WHERE Proficiency = 'On Grade Level' AND Campus = 'MS') AS sub
ORDER BY sub.iReady_Index;
-- Drop off from iReady 1 to iReady 2,-9%, due to the criteria for 'proficiency changing, in which the overall relative placement category 'Early On Grade Level' not being recongnized as on Grade Level
-- MS plauged with staff turnover

/* HS Campus Proficiency Analysis ELA */
SELECT
  sub.iReady_Index, 
  sub.Campus,
  sub.iReady_Admin,
  sub.Proficiency,
  sub.Percentage,
  sub.Admin_Change,
  ROUND(AVG(sub.Admin_Change) OVER (),2) AS Avg_Admin_Change
FROM
(SELECT
  iReady_Index, 
  Campus,
  iReady_Admin,
  Proficiency,
  Percentage,
  COALESCE(ROUND(Percentage - LAG( Percentage,1) OVER (ORDER BY iReady_Index),2),0) AS Admin_Change,
FROM `my-data-project-36654.iReady_Summative_Analysis.Campus_Proficiency_Anaysis`
WHERE Proficiency = 'On Grade Level' AND Campus = 'HS') AS sub
ORDER BY sub.iReady_Index;
~~~
