# SQL project analysing the wages of cybersecurity professionals from different approaches 💰

## To implement this project we use the 'InfoSec/Cyber Security Salaries Classification' Dataset from Kaggle, here you can explore it [Kaggle-Dataset](https://www.kaggle.com/datasets/whenamancodes/infoseccyber-security-salaries)

## The first stage has been the analysis 🧐 of the dataset and after that the following questions came to mind:

### 1- Does experience play a role in the recruitment of remote work profiles?
### 2- How does the size of the company impact on wages?
### 3- Which are the top 5 locations in remote work? And with the higher salaries? 
### 4- What are the highest paid positions? And worst ones?
### 5- In our last question we want to analize the JR positions ('EN'/ entrey level) by company size in 2022.


# 💣 Here is the project solution, hope you like it!

### First, We create the Database 

```sql
CREATE DATABASE cibersecurity_salary;
```

### Then import CSV using the Table Data Import Wizard option of MySQL Workbench
### Select the created table

```sql
USE cibersecurity_salary;
```

### Realize an overview of the Dataset

```sql
SELECT * FROM cyber_salaries;
```

### We can now begin our analysis by answering the questions above

### 1- Does experience play a role in the recruitment of remote work profiles?

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


