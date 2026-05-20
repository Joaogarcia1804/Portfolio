# Case Study 2: Paris Airbnb Listings


## 💾 Contents
- [Task](#-task)
- [Wrangling](#-wrangling)
- [Analysis](#-analysis)



## 📚 Task
    ● Calculate metrics
    
    ● Produce Data Frames to be used in visualization



## 🧹 Wrangling

  ● Creates a table with just the listings where the city is Paris

```python
import pandas as pd
import numpy as np
listings = pd.read_csv('Listings.csv', encoding='latin-1')

Paris_data = listings.loc[listings['city'] == 'Paris', ['host_since', 'neighbourhood', 'city', 'accommodates', 'price']].copy()

Paris_data.to_csv('ptable.csv', index=False)
```

  ● Updates the table to have no missing values

```python
ptable.dropna(inplace=True)
```

  ● Updates the table to have no 0 values
  
```python
ptable =  ptable.replace({'price': 0, 'accommodates': 0}, np.nan).dropna()
```

  ● Changes the host_since column to datetime
  
  
```python
ptable['host_since'] = pd.to_datetime(ptable['host_since'], errors='coerce')
```



## 🔍 Analysis

  ● Gets the minimum, maximum and average price of the listings

```python
min_price = ptable['price'].min()
max_price = ptable['price'].max()
avg_price = round(ptable['price'].mean(), 2)
```


  ● Gets the minimum, maximum and average number of people that can be accommodated in the listings
  
```python
min_accommodates = ptable['accommodates'].min()
max_accommodates = ptable['accommodates'].max()
avg_accommodates = round(ptable['accommodates'].mean(), 2)
```


  ● Makes a summary table with the data above

```python
summary_data = {
    'Metric': ['Minimum' , 'Maximum', 'Average'],
    'Price': [min_price, max_price, avg_price],
    'Accommodates': [min_accommodates, max_accommodates, avg_accommodates]
}

print(pd.DataFrame(summary_data))
```

  ● Creates a table that groups Paris listings by neighbourhood and calculates the mean price

```python
df = pd.DataFrame(ptable.groupby('neighbourhood', as_index=False)['price'].mean().sort_values(by='price', ascending=False))

df.to_csv('paris_listings_neighbourhood.csv', index=False)
```

  ● Creates a table of listings in Elysee (the most expensive in the paris_listings_neighbourhood) grouped by the number of guests they accommodate

```python  
N_Elysee = ptable[ptable['neighbourhood'] == 'Elysee']

df = pd.DataFrame(N_Elysee.groupby('accommodates', as_index=False)['price'].mean().sort_values(by='accommodates', ascending=True))

df.to_csv('paris_listings_accomodations.csv', index=False)
```

  ● Creates a table of the yearly number and average price of the listings

```python 
## creates a new temporary column with the year of the host_since column
ptable['year'] = ptable['host_since'].dt.year

## Group the data by year, find the mean price and count of listings for each year
paris_listings_over_time = ptable.groupby('year').agg({
    'price': 'mean',
    'host_since': 'count'
})

## create a new table
paris_listings_over_time.to_csv('paris_listings_over_time.csv', index=True)
```




















