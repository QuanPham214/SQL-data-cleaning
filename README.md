#DATA CLEANING
## Project Description
This is an educational project on data cleaning and preparation using SQL. The original database in CSV format is located in the file club_member_info.csv. Here, we will explore the steps that need to be applied to obtain a cleansed version of the dataset.

### Get the 10 first rows from the table using SQL console

```sql
SELECT * 
FROM club_member_info cmi 
LIMIT 10
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

### Copy the table
#### Create new table for cleaning
```sql
-- club_member_info definition

CREATE TABLE club_member_info (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
#### Copy all values from original table
```sql
INSERT INTO club_member_info_cleaned 
SELECT * FROM club_member_info 
```

#### Trim any space in the column
```sql
UPDATE club_member_info_cleaned
SET full_name       = TRIM(full_name),
    martial_status  = TRIM(martial_status),
    email           = TRIM(email),
    phone           = TRIM(phone),
    full_address    = TRIM(full_address),
    job_title       = TRIM(job_title),
    membership_date = TRIM(membership_date);
```
#### Standardise the full name by Capitalising the first letter of the full name
```sql
UPDATE club_member_info_cleaned
SET full_name = UPPER(SUBSTR(full_name,1,1)) || LOWER(SUBSTR(full_name,2))
WHERE full_name IS NOT NULL;
```

#### Replace unsuitable age by NULL:

```sql
UPDATE club_member_info_cleaned
SET age = NULL
WHERE age NOT BETWEEN 0 AND 120;
```
#### Correct the spell of column 'martial_status' into 'marital_status'
```sql
ALTER TABLE club_member_info_cleaned
	RENAME COLUMN martial_status TO marital_status;
```
#### Standardise the values in the column 'marital_status' by uppercasing the first letter and setting NULL for missing data;
```sql
UPDATE club_member_info_cleaned
SET marital_status = CASE LOWER(marital_status)
                       WHEN 'married'   THEN 'Married'
                       WHEN 'single'    THEN 'Single'
                       WHEN 'divorced'  THEN 'Divorced'
                       WHEN 'widowed'   THEN 'Widowed'
                       ELSE NULL      
                     END;
 ```
### The final result of data cleaning - Get the first 10 rows after cleaning data
|full_name|age|marital_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|Addie lush|40|Married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|Rock cradick|46|Married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel sharvell|46|Divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|Gaylor redhole|38|Married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar|44|Single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann kenealy|41|Married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|Joete cudiff|51|Divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|Mendie alexandrescu|46|Single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
|Fey kloss|52|Married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

```sql
SELECT * 
FROM club_member_info_cleaned
LIMIT 10; 
```
