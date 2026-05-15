# Case Study 1: Used Cars Market


## 💾 Contents
- [Business Task](#-business-task) 
- [The Data](#%EF%B8%8F-the-data)
- [Solution](#-solution)
  - [Data Cleaning](#-data-cleaning)
  - [Analysis](#-analysis)

All the data used was sourced from the following link: [here](https://www.kaggle.com/datasets/nehalbirla/vehicle-dataset-from-cardekho).


## 📚 Business Task
  ● Identify the key variables that impact the resale value of used cars from dealerships and independent sellers.

  ● Analyse market trends and consumer preferences.


## 🗂️ The Data
<img width="1063" alt="image" src="https://raw.githubusercontent.com/JoaoGarciaData/JoaoGarciaData/main/Data.png">



## ✅ Solution

### 🧹 Data cleaning

  ● Create a new table with the Brand and Model separated for further analysis while keeping the old table untouched

````sql
Create table cars_table as 
select substring_index(name, ' ', 1) as Brand,
substr(name, locate(' ',name) + 1) as Model,
year, selling_price, km_driven, fuel, seller_type, transmission, owner
from `cars_dataset`.`cars1`
`````

  ● Since the selling price is in Rupes, this converts Rupes to Euros at the current conversion rate
  
````sql
update `cars_dataset`.`cars_table`
set selling_price = round(selling_price / 110.58)
`````



### 🔍 Analysis

  ● Anual average price of a used car 

````sql
select year, avg(selling_price) as AVG_Price
from `cars_dataset`.`cars_table`
group by year
order by year
````

  ● Number of km driven with each fuel type per year

````sql
select year, fuel, round(avg(km_driven)) as avg_km_driven
from `cars_dataset`.`cars_table`
group by year, fuel
order by fuel, year
````

  ● Yearly average price by transmission

````sql
select year, transmission, round(avg(selling_price)) as avg_price
from `cars_dataset`.`cars_table`
group by year, transmission
order by year, transmission
````

  ● Comparison of yearly fuel type usage

````sql
select year, fuel, count(*) as car_count, avg(selling_price)
from `cars_dataset`.`cars_table`
group by year, fuel
order by fuel, year
````

  ● Average car price by mileage bracket

````sql
select concat_ws('-', start_km, end_km) as km_var, num_cars, avg_price
from (
	select floor(km_driven / 10000) * 10000 as start_km , floor(km_driven / 10000) * 10000 + 9990 as end_km,
 count(*) as num_cars, avg(selling_price) as avg_price
from `cars_dataset`.`cars_table`
where km_driven between 0 and 300000
group by start_km, end_km
) as results
order by start_km
````

  ● Anual average car price by brand

````sql
select Brand, year, avg(selling_price) as AVG_Price
from `cars_dataset`.`cars_table`
group by Brand, year
order by Brand, AVG_Price
````





 



