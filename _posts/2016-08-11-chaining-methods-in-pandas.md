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

<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIPS code</th>
      <th>Jurisdiction</th>
      <th>Division</th>
      <th>Precincts</th>
      <th>Total Registration</th>
      <th>Make</th>
      <th>Model</th>
      <th>Equipment Type</th>
      <th>VVPAT</th>
      <th>Accessible Use</th>
      <th>Early Voting</th>
      <th>Absentee Ballots</th>
      <th>Polling Place</th>
      <th>State</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>200000000</td>
      <td>Alaska State</td>
      <td>NaN</td>
      <td>441</td>
      <td>574441</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Hand Counted Paper Ballots</td>
      <td>NaN</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>200000000</td>
      <td>Alaska State</td>
      <td>NaN</td>
      <td>441</td>
      <td>574441</td>
      <td>Premier/Diebold (Dominion)</td>
      <td>AccuVote TSX</td>
      <td>DRE-Touchscreen</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>200000000</td>
      <td>Alaska State</td>
      <td>NaN</td>
      <td>441</td>
      <td>574441</td>
      <td>Premier/Diebold (Dominion)</td>
      <td>AccuVote OS</td>
      <td>Optical Scan</td>
      <td>NaN</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>100000000</td>
      <td>Alabama State</td>
      <td>NaN</td>
      <td>2527</td>
      <td>2986782</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>100100000</td>
      <td>Autauga County</td>
      <td>NaN</td>
      <td>17</td>
      <td>33806</td>
      <td>Election Systems &amp; Software</td>
      <td>AutoMARK</td>
      <td>Ballot Marking Device or System</td>
      <td>NaN</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



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
    .pipe(fillna, 'state', 'location')
)
```

Much cleaner, readable and maintainable.