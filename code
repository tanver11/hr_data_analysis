create database hr;

select * from hr;

#////////////////////DATA CLEANING/////////////////////////////


#column name change
alter table hr
change column ï»¿id emp_id varchar(20) null;


#safe update error solved
set sql_safe_updates = 0;



# date type change ,'y/m/d' to 'y-m-d'
UPDATE hr
SET birthdate = 
    CASE
        -- If the date is in "date/month/year" format (e.g., 10/25/1990)
        WHEN birthdate LIKE '%/%/%' THEN
            CONCAT(
                SUBSTRING_INDEX(birthdate, '/', -1), -- Year
                '-', 
                LPAD(SUBSTRING_INDEX(SUBSTRING_INDEX(birthdate, '/', 1), '/', -1), 2, '0'), -- Month
                '-', 
                LPAD(SUBSTRING_INDEX(birthdate, '/', 1), 2, '0') -- Day
            )
        -- If the date is in "date-month-year" format (e.g., 10-25-1990)
        WHEN birthdate LIKE '%-%-%' THEN
            CONCAT(
                SUBSTRING_INDEX(birthdate, '-', -1), -- Year
                '-', 
                LPAD(SUBSTRING_INDEX(SUBSTRING_INDEX(birthdate, '-', 1), '-', -1), 2, '0'), -- Month
                '-', 
                LPAD(SUBSTRING_INDEX(birthdate, '-', 1), 2, '0') -- Day
            )
        ELSE
            birthdate -- If the date is already in the desired format, leave it as is
    END;
   
  
  #column(birthdate) text datatype change to date datatype
  ALTER TABLE hr
modify column birthdate DATE;
 
select birthdate from hr;


-- check data type
describe hr;


# Now edit the hire_date column 
UPDATE hr
SET hire_date = 
    CASE
        -- If the date is in "date/month/year" format (e.g., 10/25/1990)
        WHEN hire_date LIKE '%/%/%' THEN
            CONCAT(
                SUBSTRING_INDEX(birthdate, '/', -1), -- Year
                '-', 
                LPAD(SUBSTRING_INDEX(SUBSTRING_INDEX(birthdate, '/', 1), '/', -1), 2, '0'), -- Month
                '-', 
                LPAD(SUBSTRING_INDEX(birthdate, '/', 1), 2, '0') -- Day
            )
        -- If the date is in "date-month-year" format (e.g., 10-25-1990)
        WHEN hire_date LIKE '%-%-%' THEN
            CONCAT(
                SUBSTRING_INDEX(birthdate, '-', -1), -- Year
                '-', 
                LPAD(SUBSTRING_INDEX(SUBSTRING_INDEX(birthdate, '-', 1), '-', -1), 2, '0'), -- Month
                '-', 
                LPAD(SUBSTRING_INDEX(birthdate, '-', 1), 2, '0') -- Day
            )
        ELSE
            hire_date -- If the date is already in the desired format, leave it as is
    END;
select hire_date from hr;







# now edit the termdate column(remove the time part & fill null value)
-- Update the termdate column to remove the time portion and set empty values to NULL
UPDATE hr
SET termdate = IF(termdate = '', NULL, STR_TO_DATE(termdate, '%Y-%m-%d %H:%i:%s UTC'));

select termdate from hr;

alter table hr
modify column termdate date;

alter table hr add column age int;
select * from hr;

# find out the age from birthdate column
update hr 
set age = timestampdiff(year,birthdate,curdate());








# ////////////////////DATA ANALYSIS////////////////////
-- 1. What is the gender breakdown of employees in the company?
select gender,count(*) as count from hr 
where age>=18 and termdate is null
group by gender;



-- 2. What is the race/ethnicity breakdown of employees in the company?
select race,count(*) as count from hr
where age>=18 and termdate is null
group by race
order by count desc;



-- 3. What is the age distribution of employees in the company?
SELECT MIN(age) AS min_age, MAX(age) AS max_age
FROM hr;

SELECT
    CASE
        WHEN age BETWEEN 18 AND 24 THEN '18-24'
        WHEN age BETWEEN 25 AND 30 THEN '25-30'
        WHEN age BETWEEN 31 AND 35 THEN '31-35'
        WHEN age BETWEEN 36 AND 40 THEN '36-40'
        WHEN age BETWEEN 41 AND 45 THEN '41-45'
        WHEN age BETWEEN 46 AND 50 THEN '46-50'
        WHEN age BETWEEN 51 AND 55 THEN '51-55'
        -- Add more age group ranges as needed
        ELSE '55+'  -- You can create a group for ages over 36, adjust as needed
    END AS age_group,gender,COUNT(*) AS count
FROM hr
where age>=18 and termdate is null
GROUP BY age_group,gender
ORDER BY age_group,gender;



-- 4. How many employees work at headquarters versus remote locations?
select location,count(*) as count from hr
where age >=18 and termdate is null
group by location;



-- 5. What is the average length of employment for employees who have been terminated?
SELECT AVG(DATEDIFF(termdate, hire_date))/365 AS avg_length_of_employment
FROM hr
WHERE age >=18 and termdate <=curdate() and termdate is not null;



-- 6. How does the gender distribution vary across departments and job titles?
SELECT department,gender,COUNT(*) AS count
FROM hr
where age >=18 and termdate is null
GROUP By department,gender
order by department;



-- 7. What is the distribution of job titles across the company?
select jobtitle,count(*) as count from hr
where age >=18 and termdate is null
group by jobtitle
order by jobtitle desc;



-- 8. What is the distribution of employees across locations by city and state?
select location_state,location_city,count(*) as count from hr
where  age >=18 and termdate is null
group by location_state,location_city
order by count desc;



-- 9. How has the company's employee count changed over time based on hire and term dates?
select year,hires,terminations,hires-terminations as net_change,round((hires-terminations)/hires*100,2) as net_change_percent
from
	(select 
		year(hire_date) as year,
        count(*) as hires,
        sum(case when termdate is not null and termdate <=curdate() then 1 else 0 end) as terminations
from hr
where  age >=18 
group by year(hire_date)) as subquery
order by year asc;



-- 10. What is the tenure distribution for each department?
select department,round(avg(datediff(termdate,hire_date)/365),0) as avg_tenure
From hr
where termdate <=curdate() and termdate is not null and age >=18
group by department;

# //////////////////Visualization is in github which is made by power bi/////////////////
