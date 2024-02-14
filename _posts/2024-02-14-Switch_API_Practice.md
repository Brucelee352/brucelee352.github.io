---
layout: post
title:  "Looking at the Switch: API Practice" 
date: "2024-02-14"
categories: projects
tags: [python]
---

## Introduction

In this script, I'm calling an API that houses data for Nintendo Switch titles, so the resulting data frame can be used in other applications. Specifically, I thought it would be interesting to work with the dates contained in the data to visualize everything in a time series. 

...

### Setup & Extracting the data  
 
```python
# Import appropriate packages:
import requests
import json
import pandas as pd
import numpy as np
import datetime

#Set URL object for API 
url = ('https://api.sampleapis.com/switch/games')

# Get API Request
res = requests.get(url)
```

The first thing I'll do here is import then needed packages, set the URL as an object to reference later and make a request to the API to pull that raw data into an object. 

...


### Working with the JSON 

```python
# Render API response to JSON
json = res.json()

# Parse JSON to data frame
data = pd.json_normalize(json, 
                         sep=' -> ',
                         max_level=1)

for col in data.columns:
    if isinstance(data[col].dropna().iloc[0], list):  
        data[col] = data[col].apply(lambda x: ', '.join(x) if x else None)

# Rename columns
data = data.rename(columns={'name': 'title', 'releaseDates -> Japan': 'JP_releaseDate', 'releaseDates -> NorthAmerica': 'US_releaseDate', 
                            'releaseDates -> Europe': 'EU_releaseDate', 'releaseDates -> Australia': 'AUS_releaseDate'})
```

Here in this snippet, I then call the JSON package to convert the raw data into JSON, which I'm storing as its own object. I then normalize everything, and then apply a loop that grabs the first entry in the lists within the JSON to output only the data contained in the JSON, with no other characters. 

Afterwards, I renamed the outputted columns for readabilities sake. 

...


### Dates, I don't say that because its Valentine's Day 

```python
# Specify date columns as an object
date_cols = ["JP_releaseDate", "US_releaseDate", "EU_releaseDate", "AUS_releaseDate"]

# Specify non-date values and then replace them with NaNs and actual formatted dates 
non_date_values = ['Unreleased', 'TBA', '2020', 'Q3 2020']

data[date_cols] = data[date_cols].replace(non_date_values, np.nan)

for col in date_cols:
    data[col] = pd.to_datetime(data[col], errors='coerce', format='%B %d, %Y')
```
So here I wanted to clean up the dates a bit, and to do that I ascribed the date columns as their own object. Then I had Python identify the non-date values contained in the data set, so as to replace them all with NaNs and they would be read by Python as NULL. 

Lastly, another loop uses Pandas datetime to coerce the dates into a singular format. 

---

```python 
# Check data     
data.head(10)
    
# A function to check if a value is a date or NaN
def is_date_or_nan(val):
    if pd.isna(val):
        return True
    try:
        pd.to_datetime(val, format='%Y-%m-%d')
        return True
    except ValueError:
        return False

# Check for dates in each column
for col in date_cols:
    if not data[col].apply(is_date_or_nan).all():
        print(f"{col} doesn't only have dates in it")
        break
else:
    print("All specified columns contain only dates or NaN")
    
# Write data to csv: 
data.to_csv("C:/Users/bruce/Documents/Python/switch_games.csv", index=False)
```

From there, I check the data using 'head' to read the first ten columns to check for consistency of formatting. Then I employ a function to check if a value is an actual date or a NULL value. I then iterate over the dataset using that function to check if each columns is uniform in content. 

Lastly, I wrote the resulting data set to a .csv, which can be used into Tableau, Looker, PowerBI or your favorite BI tool of choice to visualize everything and glean insight fron the data.  

---

### Outro

Theres a good amount you can do and not all ETL scripts are alike, yet they all mostly carry out similar functionalities and this is no different. With that said, the Nintendo Switch carries an impressive array of games, consider checking it out. 