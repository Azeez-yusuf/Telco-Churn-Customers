# Telco-Churn-Customers Exploration

## Project Overview
The sole aim of this project is to gain insight into the churn dataset, in-order to understand different factors affecting retainability of customers across the country

## The Raw Data
For clarity and consistency, I have already processed the raw data. This involved redefining columns such as the Customer ID and generating new Date and Time features.

## Tools and Steps
•	EXCEL; Used for:
1. Data loading and inspection
2. Data manipulations
3. Handling missing values,
4. Cleaning and
5. Formatting

•  SQL; Steps took involved:
1. Loading the refined datasets into the Database
2. Feature Engineering
3. Analysis

• Power BI; Steps took involved:
1. Transformation
2. Feature Engineering
3. Analysis
4. Visualizations

## Challenges
•	Challenge  1: Several improper casing of alphabets, a lot of misspellings, duplicates columns and rows, values errors, nulls and missing values were observed in the raw dataset.
      o	Solution: I use MS EXCEL to perform data manipulations, transformations, cleaning and formatting

•	Challenge 2: Another hurdle was loading the dataset to SQL server base, this was as a result of incorrect formatting of the CSV file to correspond with the SQL data type format
      o	Solution: This was also handled using Excel and SQL

• Challenge 3: After successive analysis in Sql, Creating successful relationships between exported files was herculian task in Power BI
      o	Solution: This was done by noting the type of relationship between the export files and applying them
      
## Feature Engineering
Feature engineering in SQL for data analysis involves strategically crafting new, informative features from existing data to enhance model performance and extract deeper insights.
Here are lists of Feature Engineering:

(1)	Add a new column named Time of the day:  To give insight of churned customers in the Morning, Afternoon and Evening. This will help answer the question on which part of the day most sales are made.

(2)	Added a new column named Day name that contains the extracted days of the week on which the given churn took place (Mon, Tue, Wed, Thur, Fri). This will help answer the question on which day of the week has the most churn.


(3)	Add a new column named Month name: that contains the extracted months of the year on which the given churn took place (Jan, Feb, Mar and so on). Help determine which month of the year has the most churn.

(4)	Add a new column named Satisfaction comments: Which was derived from Satisfactory rate, which also helps to put to meaning to customers’ emotions towards the company’s products and services.

(5)	Add a new column named Churn case: Yes; This translates to wether the customer has left. Or No;  which means customer is still on the service but not just purchasing the product and services

(6)	Add a new column named Geopolitical zones: This was gotten from Location. This helps to determine which cumulative number of churn, and which geopolitical zone has the most churn

(7)	Add a new column named Age Bracket: This was derived from Age; Also helps to know the cumulative numbers of customers that has left or stop purchasing the services

(8)	Add a new column named Usage Type:

(9)	Add a new column named Monthly Bill Package:

10	Add a new column named Usage Type:

11	Add a new column named Frequency of Purchase:


## Feature Engineering CODES

#### DataBase Creation AND Importation Process

```sql
CREATE DATABASE IF NOT EXISTS telco;
USE telco;
DROP TABLE churn;
CREATE TABLE IF NOT EXISTS churn(
								                CustomerID varchar(255),
                                Age int,
                                Gender varchar(255),
                                Location varchar(255),
                                Subscription_Length_Month int,
                                Monthly_Bill DECIMAL(20,2),
                                Total_Usage_GB DECIMAL(20,2),
                                Number_Of_Time_Purchase int,
                                Device_Used varchar(255),
                                Satisfaction_Rate int,
                                Churn int,
                                Reason_Or_Complains varchar(255),
                                Date_Deactivated DATE,
                                Time_Deactivated TIME
);

```

### FEATURE ENGINEERING
-- --------------------------------------------------------------------------------------
-- --------------------------------------------------------------------------------------
-- ----------------FEATURE ENGINEERING-----------------------

- Time of the day

```sql
SELECT
	Time_Deactivated,
    (CASE
		WHEN `Time_Deactivated` BETWEEN "00:00:00" AND "12:00:00" THEN "MORNING"
        WHEN `Time_Deactivated` BETWEEN "12:01:00" AND "16:00:00" THEN "AFTERNOON"
        ELSE "EVENING"
	END
    ) AS Time_Of_The_Day
FROM churn;

ALTER TABLE churn ADD COLUMN  Time_Of_The_Day VARCHAR(25);

UPDATE churn
SET Time_Of_The_Day = (
	CASE
		WHEN `Time_Deactivated` BETWEEN "00:00:00" AND "12:00:00" THEN "MORNING"
        WHEN `Time_Deactivated` BETWEEN "12:01:00" AND "16:00:00" THEN "AFTERNOON"
        ELSE "EVENING"
	END
);

```
-- ------------------------------------------------------------------------------
- Day of the Week

```sql

SELECT
	Date_Deactivated,
    DAYNAME(Date_Deactivated) AS Day_Name
FROM churn;


ALTER TABLE churn ADD COLUMN Day_Name VARCHAR(25);

UPDATE churn
SET Day_Name = DAYNAME(Date_Deactivated);

```
-- ----------------------------------------------------------------------------------------

- Month_Name

```sql

SELECT
	Date_Deactivated,
    MONTHNAME(Date_Deactivated) AS Month_Name
FROM churn;  

ALTER TABLE churn ADD COLUMN Month_Name VARCHAR(25);  

UPDATE churn
SET Month_Name = MONTHNAME(Date_Deactivated);

```

-- ------------------------------------------------------------------------------------------

- Geopolitical ZoneS

```sql

SELECT
	Location,
    (CASE
		WHEN `Location`  IN( 'Benue', 'Kogi', 'Kwara', 'Nasarawa', 'Niger', 'Plateau', 'Abuja') THEN "North Central"
		WHEN `Location`  IN( 'Adamawa', 'Bauchi', 'Borno', 'Gombe', 'Taraba', 'Yobe') THEN "North East"
		WHEN `Location`  IN( 'Jigawa', 'Kaduna', 'Kano', 'Katsina', 'Kebbi', 'Sokoto', 'Zamfara') THEN "North West"
		WHEN `Location`  IN( 'Abia', 'Anambra', 'Ebonyi', 'Enugu', 'Imo') THEN "South East"
		WHEN `Location`  IN( 'Akwa Ibom', 'Bayelsa', 'Cross River', 'Delta', 'Edo', 'Rivers') THEN "South South"
	ELSE "South West"
   END
   ) AS Geopolitical_Zone
FROM churn;


ALTER TABLE churn ADD COLUMN Geopolitical_Zone VARCHAR(25);

UPDATE churn
SET Geopolitical_Zone = (CASE
		WHEN `Location`  IN( 'Benue', 'Kogi', 'Kwara', 'Nasarawa', 'Niger', 'Plateau', 'Abuja') THEN "North Central"
		WHEN `Location`  IN( 'Adamawa', 'Bauchi', 'Borno', 'Gombe', 'Taraba', 'Yobe') THEN "North East"
		WHEN `Location`  IN( 'Jigawa', 'Kaduna', 'Kano', 'Katsina', 'Kebbi', 'Sokoto', 'Zamfara') THEN "North West"
		WHEN `Location`  IN( 'Abia', 'Anambra', 'Ebonyi', 'Enugu', 'Imo') THEN "South East"
		WHEN `Location`  IN( 'Akwa Ibom', 'Bayelsa', 'Cross River', 'Delta', 'Edo', 'Rivers') THEN "South South"
	ELSE "South West"
   END
);

```

-- --------------------------------------------------------------------------------------------------------------------
- Age Bracket

```sql

SELECT
	Age,
    (CASE
			WHEN `Age` BETWEEN 18 AND 24 THEN "Young Adult"
			WHEN `Age` BETWEEN 25 AND 35 THEN "Mid Adult"
			WHEN `Age` BETWEEN 36 AND 55 THEN "Old Adult"
		ELSE "Elder"
	END
    ) AS Age_Bracket
FROM churn;


ALTER TABLE churn ADD COLUMN Age_Bracket VARCHAR(25);

UPDATE churn
SET Age_Bracket = (CASE
			WHEN `Age` BETWEEN 18 AND 24 THEN "Young Adult"
			WHEN `Age` BETWEEN 25 AND 35 THEN "Mid Adult"
			WHEN `Age` BETWEEN 36 AND 55 THEN "Old Adult"
		ELSE "Elder"
	END
	);

```


-- -----------------------------------------------------------------------------------------------

- Satisfactory_Comment from Satisfaction Rate 

```sql

SELECT
	Satisfaction_Rate,
	(CASE
		WHEN `Satisfaction_Rate` = 1 THEN "Poor"
        WHEN `Satisfaction_Rate` = 2 THEN "Fair"
		WHEN `Satisfaction_Rate` = 3 THEN "Good"
        WHEN `Satisfaction_Rate` = 4 THEN "Very Good"
	  ELSE "Excellent"
	END
	) AS Satisfactory_Comment
FROM churn;

ALTER TABLE churn ADD COLUMN Satisfactory_Comment VARCHAR(25);

UPDATE churn
SET Satisfactory_Comment = (CASE
		WHEN `Satisfaction_Rate` = 1 THEN "Poor"
        WHEN `Satisfaction_Rate` = 2 THEN "Fair"
		WHEN `Satisfaction_Rate` = 3 THEN "Good"
        WHEN `Satisfaction_Rate` = 4 THEN "Very Good"
	  ELSE "Excellent"
	END
	);
    
```

-- ----------------------------------------------------------------------------------------------------

- Monthly Bill package

```sql

SELECT
	Monthly_Bill,
    (CASE
		WHEN `Monthly_Bill` BETWEEN 30 AND 45 THEN "Commercial"
        WHEN `Monthly_Bill` BETWEEN 46 AND 59 THEN "Premium"
        WHEN `Monthly_Bill` BETWEEN 60 AND 75 THEN "Business"
        WHEN `Monthly_Bill` BETWEEN 76 AND 89 THEN "First Class"
	  ELSE "Luxury"
	END
    ) AS Monthly_Bill_Package
FROM churn;


ALTER TABLE Churn ADD COLUMN Monthly_Bill_Package VARCHAR(25);

UPDATE churn
SET Monthly_Bill_Package = (CASE
		WHEN `Monthly_Bill` BETWEEN 30 AND 45 THEN "Commercial"
        WHEN `Monthly_Bill` BETWEEN 46 AND 59 THEN "Premium"
        WHEN `Monthly_Bill` BETWEEN 60 AND 75 THEN "Business"
        WHEN `Monthly_Bill` BETWEEN 76 AND 89 THEN "First Class"
	  ELSE "Luxury"
	END
	);
    
 ```
   
-- ------------------------------------------------------------------------------------------------------

- Frequency Of Purchase Class from Number of time Purchased

```sql

SELECT
	Number_Of_Time_Purchase,
    (CASE
		WHEN `Number_Of_Time_Purchase` BETWEEN 1 AND 29 THEN "E"
        WHEN `Number_Of_Time_Purchase` BETWEEN 30 AND 59 THEN "D"
        WHEN `Number_Of_Time_Purchase` BETWEEN 60 AND 89 THEN "C"
        WHEN `Number_Of_Time_Purchase` BETWEEN 90 AND 119 THEN "B"
	  ELSE "A"
	END
    ) AS Purchase_Freq_Class
FROM churn;


ALTER TABLE churn ADD COLUMN Purchase_freq_Class VARCHAR(2);

UPDATE churn
SET Purchase_Freq_Class = (CASE
		WHEN `Number_Of_Time_Purchase` BETWEEN 1 AND 29 THEN "E"
        WHEN `Number_Of_Time_Purchase` BETWEEN 30 AND 59 THEN "D"
        WHEN `Number_Of_Time_Purchase` BETWEEN 60 AND 89 THEN "C"
        WHEN `Number_Of_Time_Purchase` BETWEEN 90 AND 119 THEN "B"
	  ELSE "A"
	END
	);

```

-- --------------------------------------------------------------------------------------

- Usage Type from Total Usage

```sql

SELECT
	Total_Usage_GB,
    (CASE
		WHEN `Total_Usage_GB` BETWEEN 50 AND 89 THEN "Minimalist"
        WHEN `Total_Usage_GB` BETWEEN 90 AND 179 THEN "Light User"
        WHEN `Total_Usage_GB` BETWEEN 180 AND 299 THEN "Moderate User"
        WHEN `Total_Usage_GB` BETWEEN 300 AND 419 THEN "Heavy User"
	   ELSE "Voracious User"
	END
    ) AS Usage_Type
FROM churn;

ALTER TABLE churn ADD COLUMN Usage_Type VARCHAR(25);

UPDATE churn
SET Usage_Type = (CASE
		WHEN `Total_Usage_GB` BETWEEN 50 AND 89 THEN "Minimalist"
        WHEN `Total_Usage_GB` BETWEEN 90 AND 179 THEN "Light User"
        WHEN `Total_Usage_GB` BETWEEN 180 AND 299 THEN "Moderate User"
        WHEN `Total_Usage_GB` BETWEEN 300 AND 419 THEN "Heavy User"
	   ELSE "Voracious User"
	END
);

```

-- ------------------------------------------------------------------------------------------------
- churn 

```sql

SELECT
	churn,
    (CASE
		WHEN `churn` = 0 THEN "No"
        ELSE "YES"
      END  
    ) AS Churn_Case
FROM churn;


ALTER TABLE Churn ADD COLUMN Churn_Case VARCHAR(5); 

UPDATE churn
SET Churn_Case = (CASE
		WHEN `churn` = 0 THEN "No"
        ELSE "YES"
      END 
);

```


## Exploratory Data Analysis
This include some interesting codes/features

-- ------------------------------------ ANALYSIS ---------------------------------------------------------------------

-- --------------- OVERVIEW --------------------------------------------------

- OVERVIEW OF THE WHOLE DATA

```sql

SELECT *
FROM churn;

-- OVER ALL

SELECT
	MIN(Date_Deactivated) as Churn_Start_Date,
    MAX(Date_Deactivated) as Churn_End_Date,
	COUNT(CustomerID) as Total_Customers,
    COUNT(DISTINCT Geopolitical_Zone) as Total_Region,
    COUNT(DISTINCT Location) as Total_States,
    SUM(Subscription_Length_Month) as Gross_Sub_Length,
    SUM(Total_Usage_GB) as Gross_Usage,
    SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Generated
FROM churn;

```
 
-- ---------------------------------------------------------------------------------------------------------
-- ------------------------------ REVENUE RELATED ----------------------------------------------------------

- REVENUE ATTRITION BY REGION

```sql

SELECT
	Geopolitical_Zone,
    SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY Geopolitical_Zone
ORDER BY Total_Revenue_Loss DESC;

```

- REVENUE ATTRITION BY LOCATION

```sql

SELECT
	Location,
    SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY Location
ORDER BY Total_Revenue_Loss DESC;

-- REVENUE ATTRITION BY Year ---------------
SELECT
	YEAR(Date_Deactivated) as Year_Deactivated,
    SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY Year_Deactivated
ORDER BY Total_Revenue_Loss DESC;

```


- REVENUE ATTRITION BY Package ---------------

```sql

SELECT
	Monthly_Bill_Package AS Package,
    SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY Package
ORDER BY Total_Revenue_Loss DESC;

```

- REVENUE ATTRITION MonthYear ------------------------------------

```sql

SELECT
	DATE_FORMAT(Date_Deactivated, '%M-%Y') AS MonthYear,
     SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY MonthYear
ORDER BY Total_Revenue_Loss DESC;

```

- TOP CUSTOMERS BY REVENUE ATTRITION --------------------------------------

```sql

SELECT
	 CustomerID,
     SUM(Monthly_Bill * Subscription_Length_Month * Number_Of_Time_Purchase) as Total_Revenue_Loss
FROM churn
GROUP BY CustomerID
ORDER BY Total_Revenue_Loss DESC
LIMIT 10;

```

-- -----------------------------------------------------------------------------------------------------------------
-- ------------------------------------- CUSTOMER RELATED ------------------------------------------------------------------
-- Age and Churn Propensity ---------------------------

```sql

SELECT
	Age_Bracket,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Age_Bracket
ORDER BY Customer_Population DESC;

```

-- Gender and Churn Propensity -------------------

```sql

SELECT
	Gender,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Gender
ORDER BY Customer_Population DESC;

```
-- Year and Churn Propensity -------------------

```sql

SELECT
	YEAR(Date_Deactivated) AS Year_Deactivated,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY Year_Deactivated
ORDER BY Customer_Population DESC;

```

-- ------------Temporal Patterns in Churn (Day Name, Month Name, Time of the Day -------------------

-- Temporal Patterns in Churn by Day Name

```sql

SELECT
	Day_Name AS DayName,
    COUNT(CustomerID) as Customer_Population,
     (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY DayName
ORDER BY Customer_Population DESC;

```


-- Temporal Patterns in Churn by Month Name

```sql

SELECT
	Month_Name AS MonthName,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY MonthName
ORDER BY Customer_Population DESC;

```

-- Temporal Patterns in Churn by Time of the day

```sql

SELECT
	Time_Of_The_Day,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY Time_Of_The_Day
ORDER BY Customer_Population DESC;

```

-- Geopolitical Zone and Churn Propensity --------------------

```sql

SELECT
	Geopolitical_Zone AS Region,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY Region
ORDER BY Customer_Population DESC;

```

-- Location and Churn Propensity ----------------

```sql

SELECT
	Location AS State,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY State
ORDER BY Customer_Population DESC;

```

-- Subscription Length and Churn -------------

```sql

SELECT
	Subscription_Length_Month AS Duration,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Duration
ORDER BY Customer_Population DESC;

```

-- Monthly Bill Sensitivity to Churn ------------

```sql

SELECT
	Monthly_Bill_Package AS Package,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Package
ORDER BY Customer_Population DESC;

```

-- Total Usage Type and Churn --------------------

```sql

SELECT
	Usage_Type AS Consumption_Rate_Type,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Consumption_Rate_Type
ORDER BY Customer_Population DESC;

```

-- Device Used and Churn -------------

```sql

SELECT
	Device_Used,
    COUNT(CustomerID) as Customer_Population,
    (COUNT(CustomerID)/100410 * 100) as Population_Percent
FROM churn
GROUP BY Device_Used
ORDER BY Customer_Population DESC;

```

-- Satisfaction as a Churn Predictor ---------------------------

```sql

SELECT
	Satisfactory_Comment,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Satisfactory_Comment
ORDER BY Customer_Population DESC;

```

-- Reason for Churn as predictor -----------------------

```sql

SELECT
	Reason_Or_Complains AS Reason,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY Reason
ORDER BY Customer_Population DESC;

```

-- MonthYear and Churn -----------------------------------------

```sql

SELECT
	DATE_FORMAT(Date_Deactivated, '%M-%Y') AS MonthYear,
    COUNT(CustomerID) as Customer_Population
FROM churn
GROUP BY MonthYear
ORDER BY Customer_Population DESC;

```


## Results/Findings
##### Data Insight Generation for Churn Analysis
Here are at least ten potential insights you can extract from your churned customer dataset:
1.	The start date of this dataset is 1/1/2015 and the end date is 1/1/2025. Within this period a total of 100,410 customers was lost, and Revenue Attrition of approximately 6 Billion was observed within 38 locations and 6 regions
2.	North-West has the highest revenue loss of about 1.4 Billion. While Zamfara state has the highest revenue loss among States of approximately 346 million
3.	Highest gross loss was observe in 2021 of about 630 million
4.	Age and Churn Propensity: Age bracket  36years to 55years denoted by Older Adult has the highest Number of Total Customer churning Nationwide, while Mid Adult has the highest churn rate of 50.2%
5.	Gender Differences in Churn: Although there is almost no significant difference between gender churn propensity, but female tend to leave the company than the male
6.	Year and Churn Propensity: 10.17% which was Largest churn was seen in 2022. Although lowest churn rate of 0.02% was observed in 2025
7.	Temporal Patterns in Churn (Day Name, Month Name, Time of the Day: Most of our customer left the business due to their experience in the morning, which amount to 54.3% of the total churn rate.  Saturday has the highest churn rate of 14.62%. Also October has the highest churn rate of 8.7%
8.	Geopolitical Zone and Churn Propensity: 22.33% which is the highest churn rate is observed in the North-West region of the country
9.	Location-Based Churn Patterns: The  Zamfara has 5.6% churn rate, possibly due to limited network coverage reported in customer complaints.



## RECOMMENDATIONS
1.	Customer’s experience should be improved during the morning, most importantly, as the churn rate during this time is so much.
2.	Situation in the North-West region , most especially Zamfara should be looked into has they both have the highest churn of 22.3% and 5.6% respectively
3.	Improvement in device use should be looked into as there is Indifference in the device used as to churn rate
4.	Luxury Package customer should be treated with caution as the highest revenue  of  approximate 2 billion was observed here
5.	The following customers with the ID ; 202AC0028758, 202AC0058726, 202AC0076393, 202AC0077438, 202AC0076501, 202AC0016361, 202AC0025191, 202AC0010103, 202AC001660, 202AC0064663 should be contacted and possibly, recovered back into the system.
6.	A more in-depth research such as cold call, online and emails questionnaire,  is recommended as to why customers with the ratings “fair and excellent” are leaving the business
