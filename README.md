# SQL project analysing the wages of cybersecurity professionals from different approaches. 💰

## To implement this project we use the 'InfoSec/Cyber Security Salaries Classification' Dataset from Kaggle, here you can explore it [Kaggle-Dataset](https://www.kaggle.com/datasets/whenamancodes/infoseccyber-security-salaries).

## The first stage has been the analysis 🧐 of the dataset and after that the following questions came to mind:

### 1- Does experience play a role in the recruitment of remote work profiles?.
### 2- How does the size of the company impact on wages?.
### 3- Which are the top 5 locations in remote work? And with the higher salaries?.
### 4- What are the highest paid positions? And worst ones?.
### 5- In our last question we want to analize the JR positions ('EN'/ entrey level) by company size in 2022.


# 💣 Here is the project solution, hope you like it!.

### First, We create the Database. 

```sql
CREATE DATABASE cibersecurity_salary;
```

### Then import CSV using the Table Data Import Wizard option of MySQL Workbench.
### Select the created table.

```sql
USE cibersecurity_salary;
```

### Realize an overview of the Dataset.

```sql
SELECT * FROM cyber_salaries; 
```

### We can now begin our analysis by answering the questions above.

### 1- Does experience play a role in the recruitment of remote work profiles?.

### no_remote = 0 - 20%, neutral_remote = 20% - 80%, total_remote = 80 - 100%
### EN = entry level, MI = midel level, SE = senior level, EX = executive level  

```sql
SELECT 
	experience_level,
	SUM(CASE WHEN remote_ratio = 0 THEN 1 ELSE 0 END) AS no_remote,
    SUM(CASE WHEN remote_ratio = 50 THEN 1 ELSE 0 END) AS neutral_remote,
    SUM(CASE WHEN remote_ratio = 100 THEN 1 ELSE 0 END) AS total_remote
    FROM cyber_salaries
    GROUP BY experience_level
    ORDER BY no_remote DESC , neutral_remote DESC, total_remote DESC; 
```

![Captura 1 pregunta](https://user-images.githubusercontent.com/116805861/198847022-813f9c81-08d7-468d-bf1e-622b5c68b9a5.PNG)

#### Through this analysis we conclude that senior and executive levels have a high volume of remote work versus the entry and middle levels, which are more balanced.
#### Throught Tableau we go deeper in our analysis and extract the AVG Remote Ratio by each experience level (you can check it on the tableau dashboard).

### 2- How does company size impact wages?.

### S = small (-50 employees), M = medium (50 - 250 employees), L = large (+250 employees)

```sql
SELECT 
	company_size,
    AVG (salary_in_usd) AS avg_salary_in_usd
FROM cyber_salaries
GROUP BY company_size
ORDER BY avg_salary_in_usd DESC; 
```

![Captura 2 pregunta](https://user-images.githubusercontent.com/116805861/198882836-cf4d7e55-244b-4ead-9a3c-fec798c9bbf3.PNG)

#### Cyberprofessionals in medium-sized companies earn 36% more than workers in small companies. 
#### Surprisingly, however, it is not the large companies that are the best paid, but the medium-sized ones. 

### 3- What are the top 5 locations in remote work? And with the higher salaries?. 

```sql
SELECT 
	company_location,
    SUM(CASE WHEN remote_ratio IN (100, 50) THEN 1 ELSE 0 END) AS remote_work
FROM cyber_salaries
GROUP BY company_location
ORDER BY remote_work DESC
LIMIT 5; 
```

![Captura 3 pregunta 1](https://user-images.githubusercontent.com/116805861/198883234-25ffa1a6-0c44-4a72-b3e7-884ad2b9f72b.PNG)


#### United States, England, Canada, Denmark and France are top 5 in remote work field.
#### On the Tableau analysis you will see the AVG Remote Ratio of these locations. 

```sql
SELECT 
	company_location,
	AVG (salary_in_usd) AS avg_salary_in_usd
FROM cyber_salaries
GROUP BY company_location
ORDER BY avg_salary_in_usd DESC
LIMIT 5; 
```

![captura 3 pregunta 2](https://user-images.githubusercontent.com/116805861/198883274-41b3f940-5de0-4091-8984-95abdcfd5198.PNG)

#### Highest salaries are in these countries by descending order: United Arab Emirates, United States, Australia, Singapore and Switzerland. 

### 4- What are the highest paid positions? And worst ones?.

```sql
SELECT 
	job_title,
    AVG(salary_in_usd) AS avg_salary 
FROM cyber_salaries
GROUP BY job_title
ORDER BY avg_salary DESC
LIMIT 5; 
```

![Captura 4 pregunta 1](https://user-images.githubusercontent.com/116805861/198883436-01134d9d-3cd2-4c38-9b4f-cc89b6c273e5.PNG)

#### These are the top 5 paying positions: 1 Application Security Architect, 2 Staff Security Engineer 3 Threat Intelligence Response Analyst, 4 Principal Application Security Engineer, 5 Software Security Engineer.


```sql
SELECT 
	job_title,
    AVG(salary_in_usd) AS avg_salary 
FROM cyber_salaries
GROUP BY job_title
ORDER BY avg_salary ASC
LIMIT 5; 
```

![Captura 4 pregunta 2](https://user-images.githubusercontent.com/116805861/198883529-58114522-606b-4ca3-b544-a8dda1f09959.PNG)

#### Opposite these are the 5 lowest-paid positions: 1 Network Security Engineer, 2 Data Security Analyst 3 Cloud Security Engineering Manager, 4 Threat Hunting Lead, 5 Information Security Compliance Analyst.
#### You can explore all positions on the Tableau Dashboard.

### 5- In our last question we want to analize the JR positions ('EN'/ entrey level) by company size in 2022. 
### I'm currently seeking a Jr position job in Data Analyst / BI so I thought it was interesting to analize that by company size. 
### 'If you know some opportunity I'm grateful to hear about it 😉', now, go on with the analysis!.

```sql
SELECT
	company_size,
	COUNT(experience_level) AS jr_positions
FROM cyber_salaries
WHERE experience_level = 'EN' AND work_year = '2022' 
GROUP BY company_size
ORDER BY jr_positions DESC;
```

![Captura 5 pregunta](https://user-images.githubusercontent.com/116805861/198883774-0f8c563b-2e75-4377-aa18-886190987a87.PNG)

#### As we expect, Large companies have more Jr opportunities, about x5 against Small ones (50 compared to 12). 
#### You can check all experience levels by company size on Tableau.


## Here you can check 🧐 the Tableau Dashboards developed for this project: [Tableau-Project](https://public.tableau.com/app/profile/albertogarciagarcia/viz/SQLCyber_Salary_Project/OverallAnalysis).
