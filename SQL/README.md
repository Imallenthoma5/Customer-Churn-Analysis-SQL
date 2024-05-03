# Customer-Churn-Analysis-SQL

SELECT * FROM Customer_churn_dataset.customer_churn_dataset_1;
USE Customer_churn_dataset;

RENAME TABLE Customer_churn_dataset.customer_churn_dataset_1 TO Customer_churn_dataset;

SELECT * FROM Customer_churn_dataset;
SELECT COUNT(*) AS TotalCustomers FROM Customer_churn_dataset.customer_churn_dataset_1;

-- For Counting Total Number of Customers
SELECT COUNT(*) AS TotalCustomers FROM Customer_churn_dataset;
#Total Number of Customers is 500

-- For Counting Total Churn Rate
SELECT 
    SUM(CASE WHEN Churn_status = 'Yes' THEN 1 ELSE 0 END) AS ChurnedCustomers,
    COUNT(*) AS TotalCustomers,
    (SUM(CASE WHEN Churn_status = 'Yes' THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS ChurnRate
FROM Customer_churn_dataset;
#That is, a total of 105 Customers out of 500 Customers dropped out (Churned). Churn Rate is 0.21 or 21%.

-- For Getting the average age of those who have Churned Out (Dropped Out)
SELECT AVG(Age) AS AverageAge
FROM Customer_churn_dataset
WHERE Churn_status = 'Yes';
#The Average age of all churned customers is 50.8952 Years 

-- To Discover the most common contract types among churned customers
SELECT Contract_type, COUNT(*) AS Count
FROM Customer_churn_dataset
WHERE Churn_status = 'Yes'
GROUP BY Contract_type
ORDER BY COUNT(*) DESC;
#There are Totally 56 Monthly Contracts and 49 Yearly Contracts who have Churned Out (Dropped Out)

-- To Analyze the distribution of monthly charges among churned customers
SELECT
    MIN(Monthly_Charges) AS MinCharge,
    MAX(Monthly_Charges) AS MaxCharge,
    AVG(Monthly_Charges) AS AverageCharge
FROM
    Customer_churn_dataset
WHERE
    Churn_status = 'Yes';
#The Minimum Monthly Charge is seen to be $1.17. The Maximum Monthly Charge is seen to be $99.05. And, The Average Monthly Charge is seen to be $52.4918. (We know from Latitude and Longitude data that the places are either in USA or AUS).

-- To identify the contract types that are most prone to churn
SELECT Contract_type, COUNT(*) AS ChurnCount
FROM Customer_churn_dataset
WHERE Churn_status = 'Yes'
GROUP BY Contract_type
ORDER BY ChurnCount DESC;
#There are Totally 56 Monthly Contracts and 49 Yearly Contracts who have Churned Out (Dropped Out)

-- To identify the Types/Groups that are most prone to churn
-- Contract Type
SELECT 
    contract_type,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    contract_type;
#There are Totally 56 Monthly Contracts and 49 Yearly Contracts who have Churned Out (Dropped Out)

-- Internet Service
SELECT 
    internet_service,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    internet_service;
#There are totally 46 DSL Users, and 59 Fiber Optics Users who have Churned Out (Dropped Out) 

-- Phone Service
SELECT 
    phone_service,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    phone_service;
#There are totally 6o Users with No Phone Connection, and 45 Users with Phone Connection who have Churned Out (Dropped Out)

-- Multiple Lines
SELECT 
    multiple_lines,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    multiple_lines;
#There are totally 55 Users with No Multiple Lines Connection, and 50 Users with Multiple Lines Connection who have Churned Out (Dropped Out)

-- Streaming TV
SELECT 
    streaming_tv,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    streaming_tv;
#There are totally 60 Users with No Streaming TV Connection, and 45 Users with Streaming TV Connection who have Churned Out (Dropped Out)

-- Streaming Movies
SELECT 
    streaming_movies,
    COUNT(*) AS ChurnCount
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    streaming_movies;
#There are totally 58 Users with Streaming Movies Connection, and 47 Users with No Streaming Movies Connection who have Churned Out (Dropped Out)

-- Customers with high total charges who have churned 
SELECT 
    customer_id,
    total_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
    AND total_charges > (SELECT AVG(total_charges) FROM Customer_churn_dataset WHERE churn_status = 'Yes');
#Data Provided

-- Total Charges Distribution for churned and non-churned customers
SELECT 
    total_charges,
    COUNT(*) AS churned_customers
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    total_charges;
#Total Charges for  Churned Customers Data Provided


SELECT 
    total_charges,
    COUNT(*) AS non_churned_customers
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'No'
GROUP BY 
    total_charges;
#Total Charges for Non-Churned Customers Data Provided

-- Total Amount By Churned and Non-Churned Customers
SELECT 
    SUM(total_charges) AS total_charges_sum_churned
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes';
#The Total Amount Provided by Churned Customers is $56781.6499
    
 -- Total Amount By Non-Churned Customers
 SELECT 
    SUM(total_charges) AS total_charges_sum_non_churned
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'No';
#The Total Amount Provided by Non-Churned Customers is $204809.94

-- The Average Monthly Charges for different contract types among Churned Customers
SELECT 
    contract_type,
    AVG(monthly_charges) AS avg_monthly_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    contract_type;
#The Average Monthly Charges for Churned Monthly Subscription Customers is $52.2476 and The average Monthly Charges for Churned Yearly Subscription Customers is $52.7708

-- Customers who have both online security and online backup services and have not churned
SELECT 
    customer_id
FROM 
    Customer_churn_dataset
WHERE 
    online_security = 'Yes'
    AND online_backup = 'Yes'
    AND churn_status = 'No';
#Data Provided

--  The Most Common Combinations of Services Among Churned Customers
SELECT 
    CONCAT(
        IF(online_security = 'Yes', 'Online Security, ', ''),
        IF(online_backup = 'Yes', 'Online Backup, ', ''),
        IF(device_protection = 'Yes', 'Device Protection, ', ''),
        IF(tech_support = 'Yes', 'Tech Support, ', ''),
        IF(streaming_tv = 'Yes', 'Streaming TV, ', ''),
        IF(streaming_movies = 'Yes', 'Streaming Movies, ', '')
    ) AS ServiceCombination,
    COUNT(*) AS Count
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    ServiceCombination
ORDER BY 
    Count DESC;
#Data Provided

-- The Average Total Charges for Customers Grouped by Gender and Marital Status
SELECT 
    gender,
    marital_status,
    AVG(total_charges) AS average_total_charges
FROM 
    Customer_churn_dataset
GROUP BY 
    gender, marital_status;
#The Average Total Charges provided by Married Females: $544.48319. The Average Total Charges provided by Single Females: $533.25336. The Average Total Charges provided by Single Males: $511.65853. The Average Total Charges provided by Married Males:$498.96424

-- The Average Monthly Charges for Different age groups among churned customers
SELECT 
    CASE
        WHEN age BETWEEN 18 AND 30 THEN '18-30'
        WHEN age BETWEEN 31 AND 40 THEN '31-40'
        WHEN age BETWEEN 41 AND 50 THEN '41-50'
        WHEN age BETWEEN 51 AND 60 THEN '51-60'
        ELSE 'Over 60'
    END AS age_group,
    AVG(monthly_charges) AS average_monthly_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    age_group;
#The average monthly charges for different age groups among churned customers: Years 18-30 is $71.9689, Years 31-40 is $42.0457, Years 41-50 is $42.6864, Years 51-60 is $50.26, and Over 60 is $52.1059.

-- The average age and total charges for customers with multiple lines and online backup
SELECT 
    AVG(age) AS average_age,
    SUM(total_charges) AS total_charges
FROM 
    Customer_churn_dataset
WHERE 
    multiple_lines = 'Yes' AND online_backup = 'Yes';
#The Average age for customers with multiple lines and online backup is 49.57 Years, and total charges for customers with multiple lines and online backup is $67786.729998

-- The Contract Types with the highest churn rate among senior citizens (age 65 and over)
SELECT 
    contract_type,
    COUNT(*) AS total_customers,
    SUM(CASE WHEN churn_status = 'Yes' THEN 1 ELSE 0 END) AS churned_customers,
    (SUM(CASE WHEN churn_status = 'Yes' THEN 1 ELSE 0 END) / COUNT(*)) * 100 AS churn_rate
FROM 
    Customer_churn_dataset
WHERE 
    age >= 65
GROUP BY 
    contract_type
ORDER BY 
    churn_rate DESC;
#Among senior citizens (age 65 and over): For Monthly Subscription, there are 16 Churned Customers (Churn Rate is 30.1887) and For Yearly Subscription, there are 14 Churned Customers (Churn Rate is 23.333)

-- The Average Monthly Charges for Customers who have Multiple lines and Streaming TV
SELECT 
    AVG(monthly_charges) AS average_monthly_charges
FROM 
    Customer_churn_dataset
WHERE 
    multiple_lines = 'Yes'
    AND streaming_tv = 'Yes';
#The Average Monthly Charges for customers who have Multiple lines and Streaming TV is $53.78479

-- The Customers who have Churned and used the most Online Services
SELECT 
    customer_id,
    (CASE WHEN online_security = 'Yes' THEN 1 ELSE 0 END +
     CASE WHEN online_backup = 'Yes' THEN 1 ELSE 0 END +
     CASE WHEN device_protection = 'Yes' THEN 1 ELSE 0 END +
     CASE WHEN tech_support = 'Yes' THEN 1 ELSE 0 END) AS total_online_services
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
ORDER BY 
    total_online_services DESC
LIMIT 10;
#Data Provided

-- Gender Distribution among Customers who have Churned and are on Yearly Contracts
SELECT 
    gender,
    COUNT(*) AS count
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
    AND contract_type = 'Yearly'
GROUP BY 
    gender;
#There are 30 Females and 19 Males among Customers who have Churned and are on Yearly Contracts

-- The Average Monthly Charges and Total Charges for Customers who have Churned, Grouped by Contract Type and Internet Service Type
SELECT 
    contract_type,
    internet_service,
    AVG(monthly_charges) AS avg_monthly_charges,
    SUM(total_charges) AS total_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY 
    contract_type, 
    internet_service;
#The Average Monthly Charges and Total Charges for customers who have Churned, Grouped by Contract Type and Internet Service Type:
#Fiber Optic with Monthly Subscription: Total Charges of $19093.3099 and Monthly Charges of $50.35499
#Fiber Optic with Yearly Subscription: Total Charges of $14590.7899 and Monthly Charges of $48.3112
#DSL with Yearly Subscription: Total Charges of $13346.18 and Monthly Charges of $57.41625
#DSL with Monthly Subscription: Total Charges of $9751.37 and Monthly Charges of $55.1727 

-- The Customers who have Churned and are Not Using Online Services, and their Average Total Charges  
 SELECT 
    customer_id,
    AVG(total_charges) AS avg_total_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
    AND online_security = 'No'
    AND online_backup = 'No'
GROUP BY
    customer_id;    
#Data Provided

-- The Average Monthly Charges and Total Charges for Customers who have Churned, Grouped by the Number of Dependents
SELECT 
    dependents,
    AVG(monthly_charges) AS avg_monthly_charges,
    AVG(total_charges) AS avg_total_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes'
GROUP BY
    dependents;
#The Average Monthly Charges and Total Charges for Customers who have Churned, Grouped by the Number of Dependents: Average Monthly Charges of $59.7309 and Yearly Charges of $440.85363 for Customers with 3 dependants
#Average Monthly Charges of $44.71074 and Yearly Charges of $488.09037 for Customers with 2 dependants
#Average Monthly Charges of $56.87692 and Yearly Charges of $600.148077 for Customers with 1 dependant
#Average Monthly Charges of $50.38566 and Yearly Charges of $610.0193 for Customers with 0 dependants

-- HOW-- The Customers who have Churned, and their Contract Duration in Months (for Monthly Contracts)
SELECT 
    customer_id,
    TIMESTAMPDIFF(MONTH, contract_start_date, contract_end_date) AS contract_duration_months
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes' AND contract_type = 'Monthly';
#HOW#The Customers who have churned, and their contract duration in months (for monthly contracts)

-- The Average Age and Total Charges for Customers who have Churned, grouped by Internet Service and Phone Service
SELECT
    internet_service,
    phone_service,
    AVG(age) AS average_age,
    AVG(total_charges) AS average_total_charges
FROM
    Customer_churn_dataset
WHERE
    churn_status = 'Yes'
GROUP BY
    internet_service,
    phone_service;
#Average Age is 51.47 Years and Average Total Charges is $601.9764706 for Fiber Optic Customers with No Phone Service
#Average Age is 52.56 Years and Average Total Charges is $528.676 for Fiber Optic Customers with Phone Service
#Average Age is 46.62 Years and Average Total Charges is $498.6569 for DSL Customers with No Phone Service
#Average Age is 53.4 Years and Average Total Charges is $506.6235 for DSL Customers with Phone Service

-- Customers with the highest monthly charges in each contract type
SELECT 
    contract_type,
    customer_id,
    monthly_charges AS highest_monthly_charge
FROM 
    Customer_churn_dataset AS c1
WHERE 
    monthly_charges = (
        SELECT 
            MAX(monthly_charges) 
        FROM 
            Customer_churn_dataset AS c2 
        WHERE 
            c1.contract_type = c2.contract_type
    );
# Customer_ID 125172 has the highest monthly charges of $99.66 in Monthly Contract And Customer_ID 342173 has the highest monthly charges of $99.6 in Yearly Contract

-- CHECK-- Customers who have Churned and the Average Monthly Charges compared to the Overall Average
SELECT 
    churn_status,
    AVG(monthly_charges) AS avg_monthly_charges,
    (SELECT AVG(monthly_charges) FROM Customer_churn_dataset) AS overall_avg_monthly_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes';
# Average Monthly Charges for Monthly Subscription of Churned Customers is $51.53558 and the Average Monthly Charges for all Churned Customers is $52.49181

-- CHECK-- Customers who have churned and their cumulative total charges over time
SELECT 
    customer_id,
    churn_status,
    total_charges,
    SUM(total_charges) OVER (PARTITION BY customer_id ORDER BY customer_id) AS cumulative_total_charges
FROM 
    Customer_churn_dataset
WHERE 
    churn_status = 'Yes';
#Data Provided

-- Stored Procedure to Calculate Churn Rate
DELIMITER //

CREATE PROCEDURE CalculateChurnRate()
BEGIN
    DECLARE total_customers INT;
    DECLARE churned_customers INT;
    DECLARE churn_rate DECIMAL(10, 2);

    -- Calculate total customers
    SELECT COUNT(*) INTO total_customers FROM Customer_churn_dataset;

    -- Calculate churned customers
    SELECT COUNT(*) INTO churned_customers FROM Customer_churn_dataset WHERE churn_status = 'Yes';

    -- Calculate churn rate
    IF total_customers > 0 THEN
        SET churn_rate = (churned_customers / total_customers) * 100;
    ELSE
        SET churn_rate = 0;
    END IF;

    -- Display churn rate
    SELECT churn_rate AS ChurnRate;
END//

DELIMITER ;

-- Stored Procedure to Identify High-Value Customers at Risk of Churning.

DELIMITER //

CREATE PROCEDURE IdentifyHighValueCustomersAtRisk()
BEGIN
    DECLARE high_value_customers CURSOR FOR
        SELECT customer_id, total_charges, monthly_charges
        FROM Customer_churn_dataset
        WHERE churn_status = 'Yes' AND total_charges > (SELECT AVG(total_charges) FROM Customer_churn_dataset);

    DECLARE done INT DEFAULT FALSE;
    DECLARE customer_id INT;
    DECLARE total_charges DECIMAL(10, 2);
    DECLARE monthly_charges DECIMAL(10, 2);

    -- Declare temporary table to store results
    CREATE TEMPORARY TABLE HighValueCustomersAtRisk (
        customer_id INT,
        total_charges DECIMAL(10, 2),
        monthly_charges DECIMAL(10, 2)
    );

    OPEN high_value_customers;

    read_loop: LOOP
        FETCH high_value_customers INTO customer_id, total_charges, monthly_charges;
        IF done THEN
            LEAVE read_loop;
        END IF;
        
        -- Insert high-value customer into temporary table
        INSERT INTO HighValueCustomersAtRisk VALUES (customer_id, total_charges, monthly_charges);
    END LOOP;

    CLOSE high_value_customers;

    -- Display identified high-value customers at risk
    SELECT * FROM HighValueCustomersAtRisk;
END//

DELIMITER ;
