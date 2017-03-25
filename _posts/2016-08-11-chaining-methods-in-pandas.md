---
layout: post
title: "Chaining Methods in Pandas"
date:   2016-08-13 13:00:01 +0800
categories: python data analysis pandas pipe
---

You may access the dataset through this [link](https://opendata.socrata.com/api/views/d2kg-fyem/rows.csv?accessType=DOWNLOAD).

```python
import pandas as pd
import re

# Get dataset
csv_url = 'https://opendata.socrata.com/api/views/d2kg-fyem/rows.csv?accessType=DOWNLOAD'
data = pd.read_csv(csv_url)
data.head()
```


```
FIPS      Jurisdiction Division Precincts Total        Make            Model    Equipment       VVPAT Accessible Early  Absentee Polling State 
code                                      Registration                          Type                  Use        Voting Ballots  Place

---       ---          ---      ---       ---          ---             ---      ---             ---   ---        ---    ---      ---     ---   

200000000 Alaska State NaN      441       574441       NaN             NaN      Hand            NaN   No         Yes    Yes      Yes     NaN
                                                                                Counted
                                                                                Paper
                                                                                Ballots

200000000 Alaska State NaN      441       574441       Premier/Diebold AccuVote DRE-Touchscreen Yes   Yes        Yes    No       No      NaN
                                                       (Dominion)      TSX

200000000 Alaska State NaN      441       574441       Premier/Diebold AccuVote Optical Scan    NaN   No         Yes    Yes      Yes     NaN
                                                       (Dominion)      OS

100000000 Alabama      NaN      2527      2986782      NaN             NaN      NaN             NaN   No         No     No       No      NaN
          State

100100000 Autauga      NaN      17        33806        Election        AutoMARK Ballot Marking  NaN   Yes        No     No       No      NaN
          County                                       Systems &                Device or
                                                       Software                 System
```




Dataset contains the locations of electronic voting machines in the United States. 

Supposed we process the dataset. It is common to have a code that looks like this


```python
def tidy_string(string):
    string = string.lower()
    string = re.sub(r'[^\w]+', '_', string)
    return string

processed_data = data.copy()
processed_data.columns = map(tidy_string, processed_data.columns)
processed_data = processed_data.reset_index()
processed_data['location'] = processed_data['jurisdiction'].str.split().str.get(0)
processed_data['state'] = processed_data['state'].fillna(processed_data['location'])
```

What I did here is 

1. clean the column names
2. add surrogate key
3. extract location from jurisdiction
4. fill missing state

We can improve this, by using pandas feature called `pipe` and `assign`


```python
def fillna(df, missing, filled):
    df[missing] = df[missing].fillna(df[filled])
    return df

processed_data = (data
    .copy()
    .rename(columns=tidy_string)
    .reset_index()
    .assign(location=lambda x: x['jurisdiction'].str.split().str.get(0))
    .pipe(fillna, 'state', 'location'))
```

Much cleaner, readable and maintainable.