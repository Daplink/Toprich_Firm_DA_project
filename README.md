# Toprich_Firm_DA_project
Providing explorative Data Analysis to boost company productivity and increase efficiency

### Project Overview

To improve the effectiveness of business processes companies are focussing on collecting data. Data analytics companies enable businesses to analyze the acquired data and use them as required. Data analytics services can assist in product development. Identifying potential market gaps and improving operational efficiency. etc


### Aim of the project

The aim of the project is to scrape Goodfirms website for details about Topdata Analytics Companies. These details include review, rating, year founded, location etc

### Data Source

The source of the scraped data is from the GoodFirms website.

[Check website link](https://www.goodfirms.co/big-data-analytics/data-analytics)

### Road map

### Stage 1

- Download Page
- Open page 
- Get details
- Extract details
- Make a dataframe


### Stage 2 (code Refactoring):

1. Change loops to Function
2. Take Functions to another file
3. import functions and extract
4. make a dataframe


### Libraries

```
import requests
from bs4 import BeautifulSoup
import pandas as pd
import plotly.express as px
import plotly.offline as po
po.init_notebook_mode(connected=True)
```

### Scraping the first web page

```
url = ('https://www.goodfirms.co/big-data-analytics/data-analytics')
```

### Use request library to get the info
```
r = requests.get(url)
print(r)
```

### Use BeautifulSoup to extract and use find_all function to find the attribute

```
bs = BeautifulSoup(r.text,'lxml')

firm_names = bs.find_all('span', {'itemprop': "name"})
firm_motors = bs.find_all('p', {'class': "profile-tagline"})
firm_reviews = bs.find_all('span', {'class': "listinv_review_label"})
progress_value = bs.find_all('div', {'class': "circle-progress-value"})
firm_prices = bs.find_all('div', {'class': "firm-pricing"})
firm_emps = bs.find_all('div', {'class': "firm-employees"})
firm_years = bs.find_all('div', {'class': "firm-founded"})
firm_locations = bs.find_all('div', {'class': "firm-location"})







