# Toprich_Firm_DA_project
Providing explorative Data Analysis to boost company productivity and increase efficiency

## Table of Content

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Road MAp](#road-map)
- [Libraries](#libraries)
- [Changing Datatype](#changing-datatype)

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


### FUNCTIONS TO EXTRACT DETAILS

```
def extract_detail(tag_lst):
    lst = [tag.text for tag in tag_lst]
    return pd.Series(lst)

def extract_progress_values(tag_lst):
    service_pct = [percent[1].text for percent in enumerate(tag_lst) if percent[0]%2 ==0]
    platform_pct = [percent[1].text for percent in enumerate(tag_lst) if percent[0]%2 ==1]
    return pd.Series(service_pct), pd.Series(platform_pct)
```

### EXTRACT DETAILS

```
names = extract_detail(firm_names[3:])
motors = extract_detail(firm_motors)
reviews = extract_detail(firm_reviews)
ser, pct = extract_progress_values(progress_value)
prices = extract_detail(firm_prices)
emps = extract_detail(firm_emps)
years = extract_detail(firm_years)
locations = extract_detail(firm_locations)
```

### PUT DETAILS IN A DATAFRAME

### EMPTY DATAFRAME

```
df1 = pd.DataFrame()
```

### Creating columns with extracted details

```
df1['firm_name'] = names
df1['firm_motors'] = motors
df1['firm_review'] = reviews
df1['service_pct'] = ser
df1['platform_pct'] = pct
df1['firm_price'] = prices
df1['firm_employee'] = emps
df1['firm_founded'] = years
df1['firm_location'] = locations

df1.head()
```

### FOR PAGE TWO HERE WE GO;

```
url2 = ('https://www.goodfirms.co/big-data-analytics/data-analytics?page=2')
s = requests.get(url2)
bs2 = BeautifulSoup(s.text,'lxml')
```

### LOCATION DETAILS

```
firm_names = bs2.find_all('span', {'itemprop': "name"})
firm_motors = bs2.find_all('p', {'class': "profile-tagline"})
firm_reviews = bs2.find_all('span', {'class': "listinv_review_label"})
progress_value = bs2.find_all('div', {'class': "circle-progress-value"})
firm_prices = bs2.find_all('div', {'class': "firm-pricing"})
firm_emps = bs2.find_all('div', {'class': "firm-employees"})
firm_years = bs2.find_all('div', {'class': "firm-founded"})
firm_locations = bs2.find_all('div', {'class': "firm-location"})
```

### EXTRACT DETAILS

```
names2 = extract_detail(firm_names[3:])
motors = extract_detail(firm_motors)
reviews = extract_detail(firm_reviews)
ser, pct = extract_progress_values(progress_value)
prices = extract_detail(firm_prices)
emps = extract_detail(firm_emps)
years = extract_detail(firm_years)
locations = extract_detail(firm_locations)
```

#PUT DETAILS IN A DATAFRAME,,


df2 = pd.DataFrame()

#creating columns with extracted details
df2['firm_name'] = names2
df2['firm_motors'] = motors
df2['firm_review'] = reviews
df2['service_pct'] = ser
df2['platform_pct'] = pct
df2['firm_price'] = prices
df2['firm_employee'] = emps
df2['firm_founded'] = years
df2['firm_location'] = locations
```

### PREVIEW

```
df2.head()
#creating columns with extracted details
df2['firm_name'] = names2
df2['firm_motors'] = motors
df2['firm_review'] = reviews
df2['service_pct'] = ser
df2['platform_pct'] = pct
df2['firm_price'] = prices
df2['firm_employee'] = emps
df2['firm_founded'] = years
df2['firm_location'] = locations

### PREVIEW
df2.head()
```

### COMBINING ALL DATAFRAME
```
all_df = pd.concat([df1, df2], ignore_index=True)
all_df
```

### SAVE TO A CSV FILE

```
#DATA   CLEANING

df = all_df.copy()
df.head(5)
```

### insert another column and modifications

```
df['firm_review'].apply(lambda x: x.split()[0])[:5]
#adding star rating column
df.insert(2, 'star_rating', df['firm_review'].apply(lambda x: x.split()[0]))
#addinh review column
df.insert(3, 'review', df['firm_review'].apply(lambda x: x.split()[1].strip('(')))
#renaming sewrvice and platform column
df.rename(columns={'service_pct':'service_pct(%)'}, inplace=True)
df['service_pct(%)'] = df['service_pct(%)'].apply(lambda x: x.strip('%'))
df.rename(columns={'platform_pct':'platform_pct(%)'}, inplace=True)
df['platform_pct(%)'] = df['platform_pct(%)'].apply(lambda x: x.strip('%'))
                           
df['firm_price'] = df['firm_price'].apply(lambda x: x.strip('\n'))
df['firm_location'] = df['firm_location'].apply(lambda x: x.strip('\n'))
                           
df.drop(columns='firm_review', inplace=True)
df.head(5)
```

### Extract the cleaned data into CSV file

```
df.to_csv('clean_extraction.csv')
```

### CHANGING TEXT FORMAT

```
# CHECK THE PRESENT FORMAT
df.info()
```


#### Changing Datatype

```
df['star_rating'] = df['star_rating'].astype('float')
df['review'] = df['review'].astype('int')
df['service_pct(%)'] = df['service_pct(%)'].astype('int')
df['platform_pct(%)'] = df['platform_pct(%)'].astype('int')
```

#### checking for top 5 star review and rating

```
top5_sta_rev = df.sort_values(by=['star_rating', 'review'], ascending=False)[:5]
top5_sta_rev
```

#### Visualise data in Histogram

```
px.histogram(df, 'star_rating', width=500, title='Star Rating Distribution')
````

#### Another visualisation

```
px.bar(top5_sta_rev, 'firm_name', ['star_rating', 'review'], width=700, title= 'Top 5 firm based on star rating review')
```

#### THE ABOVE SHOW TOP 5 FIRMS WITH A 5 STAR RATING AND HIGHEST REVIEW

```
top5_sta_ser = df.sort_values(by=['star_rating', 'service_pct(%)'], ascending=False)[:5]

px.bar(top5_sta_ser, 'firm_name', ['star_rating', 'service_pct(%)'], width=600, title='Top 5 firms based on star rating and service percent')

```

### Sort VAlue by firm founded

```
df.sort_values('firm_founded')[:5]
``

### Visualize Data in Histogram

```
px.bar(x=countries_cnt.values, y=countries_cnt.index, labels={'x': 'count', 'y':''}, width=800, title='country frequency', height=800)
```

### Summary  

- The webpage from Goodfirm was successfully accessed and downloaded using the Request library
- BeautifulSoup was used to locate and extract details fro  the downloaded html file
- The extracted details was convert to dataFrame using pandas
- The file was cleaned and changed to the right datatype
- The cleaned data was explored for some insight
