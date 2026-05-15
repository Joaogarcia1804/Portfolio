# Case Study 1: Used Cars Market


## Contents
- [Business Task](#Business-task)
- [The Data](#the-data)
- [Solution](#solution)
  - [Data Cleaning](#data-cleaning)
  - [Analysis](#analysis)


## 📚 Business Task
  ● Identify the key variables that impact the resale value of used cars from dealerships and independent sellers.

  ● Analyse market trends and consumer preferences.


## 🗂️ The Data
<img width="1063" alt="image" src="https://raw.githubusercontent.com/JoaoGarciaData/JoaoGarciaData/main/Data.png">



## Solution

### 🧹 Data cleaning

  ● Create a new table with the Brand and Model separated for further analysis while keeping the old table untouched

````sql
Create table cars_table as 
select substring_index(name, ' ', 1) as Brand,
substr(name, locate(' ',name) + 1) as Model,
year, selling_price, km_driven, fuel, seller_type, transmission, owner
from `cars_dataset`.`cars1`
`````

  ● Since the selling price is in rupes, this converts Rupes to Euros at current conversion rate
  
````sql
update `cars_dataset`.`cars_table`
set selling_price = round(selling_price / 110.58)
`````



### 🔍 Analysis

