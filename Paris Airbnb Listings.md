# Case Study 2: Paris Airbnb Listings


## 💾 Contents
  	[Task]
    [Wrangling]
    [Analysis]



## 📚 Task
    ● Calculate metrics
    
    ● Produce Data Frames to be used in visualization



## 🧹 Wrangling

    ● Creates a table with just the listings where the city is Paris

```python
import pandas as pd
import numpy as np
listings = pd.read_csv('Listings_cleaned.csv', encoding='latin-1')

Paris_data = listings.loc[listings['city'] == 'Paris', ['host_since', 'neighbourhood', 'city', 'accommodates', 'price']].copy()

Paris_data.to_csv('Paris_data.csv', index=False)
```


    ● Changes the host_since column to datetime
    ● Creates a new temporary column with just the year of the host_since column
```python
ptable['host_since'] = pd.to_datetime(ptable['host_since'], errors='coerce')

ptable['year'] = ptable['host_since'].dt.year
```





























