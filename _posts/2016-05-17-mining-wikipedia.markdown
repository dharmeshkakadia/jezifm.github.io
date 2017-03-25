---
layout: post
title: 'Mining wikipedia'
date:   2016-05-17 17:00:01 +0800
categories: wikipedia d3 data science mining scrap
dataset_link: assets/data_science_links.csv
---

For this post, I would like to perform data mining on wikipedia. I would be selecting an article then create a crawler that goes through each link up to a certain degree (5 degree) and visualize it using force layout d3.

Lets start with the [data science article][data-science]. We will gather all the links on the wiki article, then after having all the link we will loop though all the links and gather their own links. We will do it for at least 4 times. 


[data-science]: https://en.wikipedia.org/wiki/Data_science




### Notebook


```python
from BeautifulSoup import BeautifulSoup
from collections import deque
import logging
import numpy as np
import pandas as pd
import re
import urllib2
import os
from sklearn.preprocessing import LabelEncoder
```

#### Config


```python
host = 'https://en.wikipedia.org'
url_path = '/wiki/Data_science'
MAX_DEGREE = 5

# logs
logger = logging.getLogger(__name__)
logger.setLevel(logging.INFO)
handler = logging.FileHandler('scrap.log')
handler.setLevel(logging.INFO)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)
```

#### Define helper function


```python
def clean_url(url):
    return re.sub('[^\w]+', '', url)

def get_page(url):
    page = None
    file_path = 'cache/{url}'.format(url=clean_url(url))
    if not os.path.isfile(file_path):
        with open(file_path, 'w') as f:
            page = urllib2.urlopen('{host}{url_path}'.format(host=host, url_path=url_path)).read()
            f.write(page)
    with open(file_path, 'r') as f:
        page = f.read()
    return page


def get_links(url):
    page = get_page(url)
    soup = BeautifulSoup(page)
    soup.prettify()
    return [anchor['href'] 
            for anchor in soup.findAll('a', href=True) 
            if re.match(r'^/wiki/\w+$', anchor['href'])]
```

#### Initialize Variables


```python
data = []
visited_urls = []
target_urls = deque([(url_path, 0)])
```

#### Start scrapping

How it works?

In simple terms script is a web crawler, it downloads the html and any html linking to it. Connection between each pages is being saved and will be used for visualization.


```python
while target_urls:
    url, degree = target_urls.pop()    
    if degree <= MAX_DEGREE:
        logger.info('Degree: {degree}, URL: {url}'.format(degree=degree, url=url))
        visited_urls.append(url)
        for link in get_links('{host}{url}'.format(host=host, url=url)):
            data.append((url, link, degree))
            if link not in visited_urls and link not in [i for i, _ in target_urls]:
                target_urls.append((link, degree+1))
```

Display sample results


```python
pd.DataFrame(data, columns=['from', 'to', 'degree']).sample(5)
```



```
| from                               | to                            | degree | 
| ---                                |                               |    --- | 
| /wiki/Histogram                    | /wiki/Computer_programming    |      1 | 
| /wiki/Probability_models           | /wiki/Main_Page               |      1 | 
| /wiki/Machine_translation          | /wiki/Cartogram               |      1 | 
| /wiki/Forbes                       | /wiki/Harvard_Business_Review |      1 | 
| /wiki/Prasanta_Chandra_Mahalanobis | /wiki/Boxplot                 |      1 | 
```



```python
# save output
df = pd.DataFrame(data, columns=['from', 'to', 'degree'])
le = LabelEncoder()
le.fit(np.union1d(df['from'], df['to']))
df['source'] = le.transform(df['from'])
df['target'] = le.transform(df['to'])
df.to_csv('output/data_science_links.csv', index=False)
```

Download output [here]({{ page.dataset_link | relative_url }})
