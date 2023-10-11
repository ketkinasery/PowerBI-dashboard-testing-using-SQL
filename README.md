# OBJECTIVE:

## QA-TESTING 

Create a HR Database in Postgre SQL, write SQL queries and create a test document to QA the HR ANALYSTICS DASHBOARD developed in Power BI Software.

* FUNCTIONAL VALIDATION: Test each feature work as per the requirement. To verify all the filters and Action Filters on the report work as per the requirement.
* DATA VALIDATION: Check accuracy and quality of data. To match the values in Power BI report with SQL queries.
* TEST DOCUMENT: Create a Test document which will contain the screenshots and queries used to test the reports.

### SKILLS INVOLVED:

* Functional Validation
* Data Validation using SQL queries
* How to install Postgres SQL
* How to create a database, insert table in DB
* Writing SQL functions like SELECT, INSERT, COPY, WHERE, GROUP BY, ORDER BY, HAVING, CROSSTAB (), CAST (), ROUND (), SUBQURIES, DATA TYPES etc
* Create a professional Test Document


### SOFTWARE REQUIRED:
* PostgreSQL
* Power-BI

### SQL QUERIES INVOLVED:

Create Table
create table hrdata
(
	emp_no int8 PRIMARY KEY,
	gender varchar (50) NOT NULL,
	marital_status varchar (50),
	age_band varchar (50),
	age int8,
	department varchar (50),
	education varchar(50),
	education_field varchar(50),
	job_role varchar(50),
	business_travel varchar(50),
	employee_count int8,
	attrition varchar(50),
	attrition_label varchar(50),
	job_satisfaction int8,
	active_employee int8
)
### Import Data in Table Using Query
```COPY hrdata FROM 'D:\hrdata.csv' DELIMITER ',' CSV HEADER;```

### Employee Count:
```select sum(employee_count) as Employee_Count from hrdata;```

### Attrition Count:
```select count(attrition) from hrdata where attrition='Yes'; ```

### Attrition Rate:
```
select 
round (((select count(attrition) from hrdata where attrition='Yes')/ 
sum(employee_count)) * 100,2)
from hrdata;
```

### Active Employee:
```
select sum(employee_count) - (select count(attrition) from hrdata  where attrition='Yes') from hrdata;
OR
select (select sum(employee_count) from hrdata) - count(attrition) as active_employee from hrdata
where attrition='Yes';
```

### Average Age:
```select round(avg(age),0) from hrdata;```

### Attrition by Gender
```
select gender, count(attrition) as attrition_count from hrdata
where attrition='Yes'
group by gender
order by count(attrition) desc;
```

### Department wise Attrition:
```
select department, count(attrition), round((cast (count(attrition) as numeric) / 
(select count(attrition) from hrdata where attrition= 'Yes')) * 100, 2) as pct from hrdata
where attrition='Yes'
group by department 
order by count(attrition) desc;
```

### No of Employee by Age Group
```
SELECT age,  sum(employee_count) AS employee_count FROM hrdata
GROUP BY age
order by age;
```

### Education Field wise Attrition:
```
select education_field, count(attrition) as attrition_count from hrdata
where attrition='Yes'
group by education_field
order by count(attrition) desc;
```

### Attrition Rate by Gender for different Age Group
```
select age_band, gender, count(attrition) as attrition, 
round((cast(count(attrition) as numeric) / (select count(attrition) from hrdata where attrition = 'Yes')) * 100,2) as pct
from hrdata
where attrition = 'Yes'
group by age_band, gender
order by age_band, gender desc;
```

### Job Satisfaction Rating
- Run this query first to activate the ```crosstab()``` function in postgres
CREATE EXTENSION IF NOT EXISTS tablefunc;
- Then run this to get o/p-
```
SELECT *
FROM crosstab(
  'SELECT job_role, job_satisfaction, sum(employee_count)
   FROM hrdata
   GROUP BY job_role, job_satisfaction
   ORDER BY job_role, job_satisfaction'
	) AS ct(job_role varchar(50), one numeric, two numeric, three numeric, four numeric)
ORDER BY job_role;

```

### POWER BI DASHBOARD:

![image](https://github.com/ketkinasery/PowerBI-dashboard-testing-using-SQL/assets/145470599/39c187ce-fa09-45f1-96df-96f8041e2503)










