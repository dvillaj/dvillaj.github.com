---
layout:     post
title:      Pandas and Covid-19 
date:       2020-04-21 08:15:00
summary:    Data manipulation and cleaning with pandas
categories: Pandas
permalink:  /pandas-and-covid-19/
minutes:    12
---

This notebook is an example of data analysis and manipulation with **Pandas** and has beed created in [Google Colab](https://colab.research.google.com)

Enjoy it!


```python
import numpy as np
import pandas as pd
```

## The Data

To get some data I and going to download it from Data Repository by Johns Hopkins CSSE

https://github.com/CSSEGISandData/COVID-19

I first remove the folder where I am goint to store the data so I can re-execute this sentences without any problems ...


```python
!rm -rf ./COVID-19
```

The dataset is avaible in GitHub so I use the `git` command to get it


```python
!git clone https://github.com/CSSEGISandData/COVID-19.git
```

    Cloning into 'COVID-19'...
    remote: Enumerating objects: 14, done.[K
    remote: Counting objects: 100% (14/14), done.[K
    remote: Compressing objects: 100% (9/9), done.[K
    remote: Total 21425 (delta 5), reused 10 (delta 5), pack-reused 21411[K
    Receiving objects: 100% (21425/21425), 90.29 MiB | 33.30 MiB/s, done.
    Resolving deltas: 100% (11436/11436), done.


## Exporing the data



```python
!ls -lt ./COVID-19/csse_covid_19_data/csse_covid_19_daily_reports | head
```

    total 9544
    -rw-r--r-- 1 root root 317177 Apr 21 05:16 04-20-2020.csv
    -rw-r--r-- 1 root root      0 Apr 21 05:16 README.md
    -rw-r--r-- 1 root root 317954 Apr 21 05:16 04-19-2020.csv
    -rw-r--r-- 1 root root 315926 Apr 21 05:16 04-18-2020.csv
    -rw-r--r-- 1 root root 314848 Apr 21 05:16 04-17-2020.csv
    -rw-r--r-- 1 root root 312551 Apr 21 05:16 04-15-2020.csv
    -rw-r--r-- 1 root root 314226 Apr 21 05:16 04-16-2020.csv
    -rw-r--r-- 1 root root 305548 Apr 21 05:16 04-12-2020.csv
    -rw-r--r-- 1 root root 309742 Apr 21 05:16 04-13-2020.csv


Yes!!!  
We have data files ...

Perfect. Let's explore the first dataset generated ...


```python
first = pd.read_csv("./COVID-19/csse_covid_19_data/csse_covid_19_daily_reports/01-22-2020.csv")
```


```python
first.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Last Update</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Anhui</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Beijing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>14.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chongqing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fujian</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gansu</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



And one of the last ones ...


```python
last = pd.read_csv("./COVID-19/csse_covid_19_data/csse_covid_19_daily_reports/04-18-2020.csv")
```


```python
last.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIPS</th>
      <th>Admin2</th>
      <th>Province_State</th>
      <th>Country_Region</th>
      <th>Last_Update</th>
      <th>Lat</th>
      <th>Long_</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Combined_Key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>45001.0</td>
      <td>Abbeville</td>
      <td>South Carolina</td>
      <td>US</td>
      <td>2020-04-18 22:32:47</td>
      <td>34.223334</td>
      <td>-82.461707</td>
      <td>15</td>
      <td>0</td>
      <td>0</td>
      <td>15</td>
      <td>Abbeville, South Carolina, US</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22001.0</td>
      <td>Acadia</td>
      <td>Louisiana</td>
      <td>US</td>
      <td>2020-04-18 22:32:47</td>
      <td>30.295065</td>
      <td>-92.414197</td>
      <td>110</td>
      <td>7</td>
      <td>0</td>
      <td>103</td>
      <td>Acadia, Louisiana, US</td>
    </tr>
    <tr>
      <th>2</th>
      <td>51001.0</td>
      <td>Accomack</td>
      <td>Virginia</td>
      <td>US</td>
      <td>2020-04-18 22:32:47</td>
      <td>37.767072</td>
      <td>-75.632346</td>
      <td>33</td>
      <td>0</td>
      <td>0</td>
      <td>33</td>
      <td>Accomack, Virginia, US</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16001.0</td>
      <td>Ada</td>
      <td>Idaho</td>
      <td>US</td>
      <td>2020-04-18 22:32:47</td>
      <td>43.452658</td>
      <td>-116.241552</td>
      <td>593</td>
      <td>9</td>
      <td>0</td>
      <td>584</td>
      <td>Ada, Idaho, US</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19001.0</td>
      <td>Adair</td>
      <td>Iowa</td>
      <td>US</td>
      <td>2020-04-18 22:32:47</td>
      <td>41.330756</td>
      <td>-94.471059</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>Adair, Iowa, US</td>
    </tr>
  </tbody>
</table>
</div>



Can I concatenate both datasets?


```python
last.query("Country_Region == 'Spain'")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIPS</th>
      <th>Admin2</th>
      <th>Province_State</th>
      <th>Country_Region</th>
      <th>Last_Update</th>
      <th>Lat</th>
      <th>Long_</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Combined_Key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3025</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Spain</td>
      <td>2020-04-18 22:32:28</td>
      <td>40.463667</td>
      <td>-3.74922</td>
      <td>191726</td>
      <td>20043</td>
      <td>74797</td>
      <td>96886</td>
      <td>Spain</td>
    </tr>
  </tbody>
</table>
</div>




```python
pd.concat((first, last), axis = 0)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Province/State</th>
      <th>Country/Region</th>
      <th>Last Update</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>FIPS</th>
      <th>Admin2</th>
      <th>Province_State</th>
      <th>Country_Region</th>
      <th>Last_Update</th>
      <th>Lat</th>
      <th>Long_</th>
      <th>Active</th>
      <th>Combined_Key</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Anhui</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Beijing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>14.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chongqing</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fujian</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Gansu</td>
      <td>Mainland China</td>
      <td>1/22/2020 17:00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3048</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>418.0</td>
      <td>2.0</td>
      <td>69.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>West Bank and Gaza</td>
      <td>2020-04-18 22:32:28</td>
      <td>31.952200</td>
      <td>35.233200</td>
      <td>347.0</td>
      <td>West Bank and Gaza</td>
    </tr>
    <tr>
      <th>3049</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Western Sahara</td>
      <td>2020-04-18 22:32:28</td>
      <td>24.215500</td>
      <td>-12.885800</td>
      <td>6.0</td>
      <td>Western Sahara</td>
    </tr>
    <tr>
      <th>3050</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Yemen</td>
      <td>2020-04-18 22:32:28</td>
      <td>15.552727</td>
      <td>48.516388</td>
      <td>1.0</td>
      <td>Yemen</td>
    </tr>
    <tr>
      <th>3051</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>57.0</td>
      <td>2.0</td>
      <td>33.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Zambia</td>
      <td>2020-04-18 22:32:28</td>
      <td>-13.133897</td>
      <td>27.849332</td>
      <td>22.0</td>
      <td>Zambia</td>
    </tr>
    <tr>
      <th>3052</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>25.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Zimbabwe</td>
      <td>2020-04-18 22:32:28</td>
      <td>-19.015438</td>
      <td>29.154857</td>
      <td>20.0</td>
      <td>Zimbabwe</td>
    </tr>
  </tbody>
</table>
<p>3091 rows × 15 columns</p>
</div>



Ups!!! The column names don't match :-(

# Loading the data into Pandas  and cleaning it


```python
import glob
import os

files = glob.glob("./COVID-19/csse_covid_19_data/csse_covid_19_daily_reports/*.csv")
files.sort(key=os.path.getmtime)
```

We are going to:
- Create a blank Dataset to store all the data
- Load every dataset unifying the column names so we can concatenate it without any problem.
- Remove extra blank spaces from the country field
- Enrich the information with the date of the data in the correct type


```python
data = pd.DataFrame()
for file in files:  
  df = pd.read_csv(file).rename(columns = {'Province/State' : 'State', 
                        "Country/Region" : 'Country',
                        'Province_State' : 'State', 
                        "Country_Region" : 'Country',
                        'Last Update' : 'Last_Update',
                        'Confirmed' : 'ConfirmedAcum',
                        'Deaths' : 'DeathsAcum',
                        'Recovered' : 'RecoveredAcum'})
  df = df.assign(Date = pd.to_datetime(file[-14:-4], format = '%m-%d-%Y'),
                 Country = df.Country.str.strip())
  data = pd.concat((data, df), axis = 0)

```

I noticed that the country names were a little messy.   
Let's fix it ...


```python
data['Country'] = data.Country.replace({'Bahamas, The' : 'Bahamas',
                         'Congo (Brazzaville)' : 'Congo',
                         'Congo (Kinshasa)' : 'Congo',
                         "Cote d'Ivoire" : "Cote d'Ivoire",
                         "Curacao" : "Curaçao",
                         'Czech Republic' : 'Czech Republic (Czechia)',
                         'Czechia' : 'Czech Republic (Czechia)',
                         'Faroe Islands' : 'Faeroe Islands',
                         'Macau' : 'Macao',
                         'Mainland China' : 'China',
                         'Palestine' : 'State of Palestine',
                         'Reunion' : 'Réunion',
                         'Saint Kitts and Nevis' : 'Saint Kitts & Nevis',
                         'Sao Tome and Principe' : 'Sao Tome & Principe',
                         'US' : 'United States',
                         'Gambia, The' : 'Gambia',
                         'Hong Kong SAR' : 'Hong Kong',
                         'Korea, South' : 'South Korea',
                         'Macao SAR' : 'Macao',
                         'Taiwan*' : 'Taiwan',
                         'Viet Nam' : 'Vietnam',
                         'West Bank and Gaza' : 'State of Palestine'
                         })
```

I'm going to fill in the null values ​​of the 'State' and 'Admin2' fields so that I can later group the data correctly


```python
data = data.fillna({'State' : 'NA', 'Admin2' : 'NA'})
```

Finally I am going to be left alone with the columns that interest me


```python
data = data[['Date', 'Country', 'State', 'Admin2', 'ConfirmedAcum', 'DeathsAcum', 'RecoveredAcum']]
```

Let's verify the structure of the dateset ...


```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 98669 entries, 0 to 3080
    Data columns (total 7 columns):
     #   Column         Non-Null Count  Dtype         
    ---  ------         --------------  -----         
     0   Date           98669 non-null  datetime64[ns]
     1   Country        98669 non-null  object        
     2   State          98669 non-null  object        
     3   Admin2         98669 non-null  object        
     4   ConfirmedAcum  98650 non-null  float64       
     5   DeathsAcum     98228 non-null  float64       
     6   RecoveredAcum  98281 non-null  float64       
    dtypes: datetime64[ns](1), float64(3), object(3)
    memory usage: 6.0+ MB


Wait a set, I think that can be interesting have a column the the active cases. Let's create it ...


```python
data['ActiveAcum'] = data.ConfirmedAcum  - data.DeathsAcum - data.RecoveredAcum
```


```python
data.query("Country == 'Spain'").sort_values('Date', ascending = False).head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>State</th>
      <th>Admin2</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3053</th>
      <td>2020-04-20</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>200210.0</td>
      <td>20852.0</td>
      <td>80587.0</td>
      <td>98771.0</td>
    </tr>
    <tr>
      <th>3044</th>
      <td>2020-04-19</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>198674.0</td>
      <td>20453.0</td>
      <td>77357.0</td>
      <td>100864.0</td>
    </tr>
    <tr>
      <th>3025</th>
      <td>2020-04-18</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>191726.0</td>
      <td>20043.0</td>
      <td>74797.0</td>
      <td>96886.0</td>
    </tr>
    <tr>
      <th>3017</th>
      <td>2020-04-17</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>190839.0</td>
      <td>20002.0</td>
      <td>74797.0</td>
      <td>96040.0</td>
    </tr>
    <tr>
      <th>3013</th>
      <td>2020-04-16</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>184948.0</td>
      <td>19315.0</td>
      <td>74797.0</td>
      <td>90836.0</td>
    </tr>
  </tbody>
</table>
</div>



Perfect :-)

Now, I am going to group and summarize the data because I want to be sure that there is only one row per Date, Country, State and Admin2


```python
data = data.groupby(["Date", "Country", "State", "Admin2"]).agg("sum").reset_index()
```


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>State</th>
      <th>Admin2</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-01-22</td>
      <td>China</td>
      <td>Anhui</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-01-22</td>
      <td>China</td>
      <td>Beijing</td>
      <td>NA</td>
      <td>14.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-01-22</td>
      <td>China</td>
      <td>Chongqing</td>
      <td>NA</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-01-22</td>
      <td>China</td>
      <td>Fujian</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-01-22</td>
      <td>China</td>
      <td>Gansu</td>
      <td>NA</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>



# Daily Cases
I am going to enrich the data by creating new columns with the daily cases.  

First I create new columns with the cases from the previous day


```python
data = data.sort_values(['State', 'Country', 'Date']).\
            assign(ConfirmedPrevious = data.groupby(['Admin2', 'State', 'Country']).shift(1)["ConfirmedAcum"],
                   DeathsPrevious = data.groupby(['Admin2', 'State', 'Country']).shift(1)["DeathsAcum"],
                   RecoveredPrevious = data.groupby(['Admin2', 'State', 'Country']).shift(1)["RecoveredAcum"],
                   ActivePrevious = data.groupby(['Admin2', 'State', 'Country']).shift(1)["ActiveAcum"],
            ).\
            fillna({ 'ConfirmedPrevious' : 0, 'DeathsPrevious' : 0, 'RecoveredPrevious' : 0 })
```


```python
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>State</th>
      <th>Admin2</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>ConfirmedPrevious</th>
      <th>DeathsPrevious</th>
      <th>RecoveredPrevious</th>
      <th>ActivePrevious</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2599</th>
      <td>2020-02-28</td>
      <td>Canada</td>
      <td>Montreal, QC</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2713</th>
      <td>2020-02-29</td>
      <td>Canada</td>
      <td>Montreal, QC</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2834</th>
      <td>2020-03-01</td>
      <td>Canada</td>
      <td>Montreal, QC</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2961</th>
      <td>2020-03-02</td>
      <td>Canada</td>
      <td>Montreal, QC</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3103</th>
      <td>2020-03-03</td>
      <td>Canada</td>
      <td>Montreal, QC</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



After that I am going to assign the new fields subtracting the previous acum cases to the actual acum cases


```python
data = data.assign(Confirmed = data.ConfirmedAcum -  data.ConfirmedPrevious,
            Deaths = data.DeathsAcum - data.DeathsPrevious,
            Recovered = data.RecoveredAcum - data.RecoveredPrevious,
            Active = data.ActiveAcum - data.ActivePrevious
            )
```

I no longer need the fields I used to make the calculation so I can drop them


```python
data = data.drop(['ConfirmedPrevious', 'DeathsPrevious', 'RecoveredPrevious', 'ActivePrevious'], axis = 1)
```

Does the data look good?


```python
data.query("Country == 'Spain'")
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>State</th>
      <th>Admin2</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>545</th>
      <td>2020-02-01</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>612</th>
      <td>2020-02-02</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>679</th>
      <td>2020-02-03</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>749</th>
      <td>2020-02-04</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>819</th>
      <td>2020-02-05</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>83563</th>
      <td>2020-04-16</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>184948.0</td>
      <td>19315.0</td>
      <td>74797.0</td>
      <td>90836.0</td>
      <td>7304.0</td>
      <td>607.0</td>
      <td>3944.0</td>
      <td>2753.0</td>
    </tr>
    <tr>
      <th>86603</th>
      <td>2020-04-17</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>190839.0</td>
      <td>20002.0</td>
      <td>74797.0</td>
      <td>96040.0</td>
      <td>5891.0</td>
      <td>687.0</td>
      <td>0.0</td>
      <td>5204.0</td>
    </tr>
    <tr>
      <th>89647</th>
      <td>2020-04-18</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>191726.0</td>
      <td>20043.0</td>
      <td>74797.0</td>
      <td>96886.0</td>
      <td>887.0</td>
      <td>41.0</td>
      <td>0.0</td>
      <td>846.0</td>
    </tr>
    <tr>
      <th>92699</th>
      <td>2020-04-19</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>198674.0</td>
      <td>20453.0</td>
      <td>77357.0</td>
      <td>100864.0</td>
      <td>6948.0</td>
      <td>410.0</td>
      <td>2560.0</td>
      <td>3978.0</td>
    </tr>
    <tr>
      <th>95770</th>
      <td>2020-04-20</td>
      <td>Spain</td>
      <td>NA</td>
      <td>NA</td>
      <td>200210.0</td>
      <td>20852.0</td>
      <td>80587.0</td>
      <td>98771.0</td>
      <td>1536.0</td>
      <td>399.0</td>
      <td>3230.0</td>
      <td>-2093.0</td>
    </tr>
  </tbody>
</table>
<p>80 rows × 12 columns</p>
</div>



## Data By Country




So far, we have data by 3 geographical levels: Country, State and a lower level called Admin2

The problem is that not all countries have this level of information, so I will create a new dataset only with the country level data


```python
data_by_country = data.groupby(["Date", "Country"]).agg("sum").reset_index()
data_by_country = data_by_country.sort_values(['Country', 'Date'])
data_by_country.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 9112 entries, 848 to 3045
    Data columns (total 10 columns):
     #   Column         Non-Null Count  Dtype         
    ---  ------         --------------  -----         
     0   Date           9112 non-null   datetime64[ns]
     1   Country        9112 non-null   object        
     2   ConfirmedAcum  9112 non-null   float64       
     3   DeathsAcum     9112 non-null   float64       
     4   RecoveredAcum  9112 non-null   float64       
     5   ActiveAcum     9112 non-null   float64       
     6   Confirmed      9112 non-null   float64       
     7   Deaths         9112 non-null   float64       
     8   Recovered      9112 non-null   float64       
     9   Active         9112 non-null   float64       
    dtypes: datetime64[ns](1), float64(8), object(1)
    memory usage: 783.1+ KB



```python
data_by_country.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>848</th>
      <td>2020-02-24</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>886</th>
      <td>2020-02-25</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>928</th>
      <td>2020-02-26</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>977</th>
      <td>2020-02-27</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1030</th>
      <td>2020-02-28</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_by_country[data_by_country.Country == 'United States']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>2020-01-22</td>
      <td>United States</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>2020-01-23</td>
      <td>United States</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>2020-01-24</td>
      <td>United States</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>47</th>
      <td>2020-01-25</td>
      <td>United States</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>62</th>
      <td>2020-01-26</td>
      <td>United States</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>8367</th>
      <td>2020-04-16</td>
      <td>United States</td>
      <td>667801.0</td>
      <td>32916.0</td>
      <td>54703.0</td>
      <td>580182.0</td>
      <td>31449.0</td>
      <td>4591.0</td>
      <td>2607.0</td>
      <td>24251.0</td>
    </tr>
    <tr>
      <th>8551</th>
      <td>2020-04-17</td>
      <td>United States</td>
      <td>699706.0</td>
      <td>36773.0</td>
      <td>58545.0</td>
      <td>604388.0</td>
      <td>31976.0</td>
      <td>3857.0</td>
      <td>3842.0</td>
      <td>24277.0</td>
    </tr>
    <tr>
      <th>8735</th>
      <td>2020-04-18</td>
      <td>United States</td>
      <td>732197.0</td>
      <td>38664.0</td>
      <td>64840.0</td>
      <td>628693.0</td>
      <td>32491.0</td>
      <td>1891.0</td>
      <td>6295.0</td>
      <td>24305.0</td>
    </tr>
    <tr>
      <th>8919</th>
      <td>2020-04-19</td>
      <td>United States</td>
      <td>759086.0</td>
      <td>40661.0</td>
      <td>70337.0</td>
      <td>648088.0</td>
      <td>27159.0</td>
      <td>1997.0</td>
      <td>5497.0</td>
      <td>19397.0</td>
    </tr>
    <tr>
      <th>9103</th>
      <td>2020-04-20</td>
      <td>United States</td>
      <td>784326.0</td>
      <td>42094.0</td>
      <td>72329.0</td>
      <td>669903.0</td>
      <td>25211.0</td>
      <td>1433.0</td>
      <td>1992.0</td>
      <td>21786.0</td>
    </tr>
  </tbody>
</table>
<p>90 rows × 10 columns</p>
</div>



## Cases per million inhabitants



We are going to enrich the information with the number of cases per million inhabitants, so we need population data by country.

A small internet search leads me to a page that has population data for 2020:

https://www.worldometers.info/world-population/population-by-country/

It seems that this information is protected to be downloaded automatically so I have no choice but to do it manually and upload the data to a GitHub Repository:

https://github.com/dvillaj/world-population/



I load the data to the Pandas, clean it up and just maintain the field of the population 



```python
population = pd.read_excel("https://github.com/dvillaj/world-population/blob/master/data/world-popultation-2020.xlsx?raw=true", sheet_name="Data")
```


```python
population.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 235 entries, 0 to 234
    Data columns (total 11 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Country            235 non-null    object 
     1   Population (2020)  235 non-null    int64  
     2   Yearly Change      235 non-null    float64
     3   Net Change         235 non-null    int64  
     4   Density (P/Km²)    235 non-null    float64
     5   Land Area (Km²)    235 non-null    int64  
     6   Migrants (net)     201 non-null    float64
     7   Fertility Rate     201 non-null    float64
     8   Average Age        201 non-null    float64
     9   Urban Pop %        222 non-null    float64
     10  World Share        235 non-null    float64
    dtypes: float64(7), int64(3), object(1)
    memory usage: 20.3+ KB



```python
population = population.rename(columns = {
    'Population (2020)' : 'Population',
    'Yearly Change' : 'Yearly_Change',
    'Net Change' : 'Net_Change',
    'Density (P/Km²)' : 'Density',
    'Land Area (Km²)' : 'Land_Area',
    'Migrants (net)' : 'igrants',
    'Fertility Rate' : 'Fertility',
    'Average Age' : 'Mean_Age',
    'Urban Pop %' : 'Urban_Pop',
    'World Share' : 'World_Share'
})
```


```python
population = population[['Country', 'Population']]
```


```python
population.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>38928346</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>2877797</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>43851044</td>
    </tr>
    <tr>
      <th>3</th>
      <td>American Samoa</td>
      <td>55191</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>77265</td>
    </tr>
  </tbody>
</table>
</div>



Now I join the population Dataset with the country data to have the population in this dataset


```python
data_by_country = data_by_country.merge(population, how = 'left', on = 'Country')
```


```python
data_by_country.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 9112 entries, 0 to 9111
    Data columns (total 11 columns):
     #   Column         Non-Null Count  Dtype         
    ---  ------         --------------  -----         
     0   Date           9112 non-null   datetime64[ns]
     1   Country        9112 non-null   object        
     2   ConfirmedAcum  9112 non-null   float64       
     3   DeathsAcum     9112 non-null   float64       
     4   RecoveredAcum  9112 non-null   float64       
     5   ActiveAcum     9112 non-null   float64       
     6   Confirmed      9112 non-null   float64       
     7   Deaths         9112 non-null   float64       
     8   Recovered      9112 non-null   float64       
     9   Active         9112 non-null   float64       
     10  Population     8783 non-null   float64       
    dtypes: datetime64[ns](1), float64(9), object(1)
    memory usage: 854.2+ KB



```python
data_by_country.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-02-24</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-02-25</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-02-26</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-02-27</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-02-28</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
    </tr>
  </tbody>
</table>
</div>



And finally I calculate the number of cases per million inhabitants


```python
data_by_country = data_by_country.assign(ConfirmedAcum_Millon = data_by_country.ConfirmedAcum / data_by_country.Population * 1000000)
data_by_country.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Population</th>
      <th>ConfirmedAcum_Millon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-02-24</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
      <td>0.025688</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-02-25</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
      <td>0.025688</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-02-26</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
      <td>0.025688</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-02-27</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
      <td>0.025688</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-02-28</td>
      <td>Afghanistan</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>38928346.0</td>
      <td>0.025688</td>
    </tr>
  </tbody>
</table>
</div>



## Rankins

I'm going to create a dataset of last day's cases. 

The goal is to get a set of rankings that tell me the countries with the most cases

So I need a variable that contains the last date of the dataset


```python
last_day = list(data_by_country.Date.sort_values(ascending = False))[0]
last_day
```




    Timestamp('2020-04-20 00:00:00')



Now I can filter the data by this date


```python
last_day_data = data_by_country[data_by_country.Date == last_day]
last_day_data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 184 entries, 56 to 9104
    Data columns (total 12 columns):
     #   Column                Non-Null Count  Dtype         
    ---  ------                --------------  -----         
     0   Date                  184 non-null    datetime64[ns]
     1   Country               184 non-null    object        
     2   ConfirmedAcum         184 non-null    float64       
     3   DeathsAcum            184 non-null    float64       
     4   RecoveredAcum         184 non-null    float64       
     5   ActiveAcum            184 non-null    float64       
     6   Confirmed             184 non-null    float64       
     7   Deaths                184 non-null    float64       
     8   Recovered             184 non-null    float64       
     9   Active                184 non-null    float64       
     10  Population            178 non-null    float64       
     11  ConfirmedAcum_Millon  178 non-null    float64       
    dtypes: datetime64[ns](1), float64(10), object(1)
    memory usage: 18.7+ KB



```python
last_day_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Population</th>
      <th>ConfirmedAcum_Millon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>2020-04-20</td>
      <td>Afghanistan</td>
      <td>1026.0</td>
      <td>36.0</td>
      <td>135.0</td>
      <td>855.0</td>
      <td>30.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>23.0</td>
      <td>38928346.0</td>
      <td>26.356116</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2020-04-20</td>
      <td>Albania</td>
      <td>584.0</td>
      <td>26.0</td>
      <td>327.0</td>
      <td>231.0</td>
      <td>22.0</td>
      <td>0.0</td>
      <td>13.0</td>
      <td>9.0</td>
      <td>2877797.0</td>
      <td>202.933007</td>
    </tr>
    <tr>
      <th>155</th>
      <td>2020-04-20</td>
      <td>Algeria</td>
      <td>2718.0</td>
      <td>384.0</td>
      <td>1099.0</td>
      <td>1235.0</td>
      <td>89.0</td>
      <td>9.0</td>
      <td>52.0</td>
      <td>28.0</td>
      <td>43851044.0</td>
      <td>61.982561</td>
    </tr>
    <tr>
      <th>205</th>
      <td>2020-04-20</td>
      <td>Andorra</td>
      <td>717.0</td>
      <td>37.0</td>
      <td>248.0</td>
      <td>432.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>-10.0</td>
      <td>77265.0</td>
      <td>9279.751505</td>
    </tr>
    <tr>
      <th>237</th>
      <td>2020-04-20</td>
      <td>Angola</td>
      <td>24.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>32866272.0</td>
      <td>0.730232</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9011</th>
      <td>2020-04-20</td>
      <td>Vietnam</td>
      <td>268.0</td>
      <td>0.0</td>
      <td>214.0</td>
      <td>54.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>-12.0</td>
      <td>97338579.0</td>
      <td>2.753276</td>
    </tr>
    <tr>
      <th>9027</th>
      <td>2020-04-20</td>
      <td>Western Sahara</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>597339.0</td>
      <td>10.044548</td>
    </tr>
    <tr>
      <th>9038</th>
      <td>2020-04-20</td>
      <td>Yemen</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>29825964.0</td>
      <td>0.033528</td>
    </tr>
    <tr>
      <th>9072</th>
      <td>2020-04-20</td>
      <td>Zambia</td>
      <td>65.0</td>
      <td>3.0</td>
      <td>35.0</td>
      <td>27.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>18383955.0</td>
      <td>3.535692</td>
    </tr>
    <tr>
      <th>9104</th>
      <td>2020-04-20</td>
      <td>Zimbabwe</td>
      <td>25.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>20.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14862924.0</td>
      <td>1.682038</td>
    </tr>
  </tbody>
</table>
<p>184 rows × 12 columns</p>
</div>



Let's assign new columns with the most interesting rankins ...


```python
last_day_data = last_day_data.assign(
    Rank_ConfirmedAcum = last_day_data.ConfirmedAcum.rank(),
    Rank_Confirmed = last_day_data.Confirmed.rank(),
    Rank_ActiveAcum = last_day_data.ActiveAcum.rank(),
    Rank_Active = last_day_data.Active.rank(),
    Rank_ConfirmedAcum_Millon = last_day_data.ConfirmedAcum_Millon.rank()
)
last_day_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
      <th>DeathsAcum</th>
      <th>RecoveredAcum</th>
      <th>ActiveAcum</th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
      <th>Active</th>
      <th>Population</th>
      <th>ConfirmedAcum_Millon</th>
      <th>Rank_ConfirmedAcum</th>
      <th>Rank_Confirmed</th>
      <th>Rank_ActiveAcum</th>
      <th>Rank_Active</th>
      <th>Rank_ConfirmedAcum_Millon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>2020-04-20</td>
      <td>Afghanistan</td>
      <td>1026.0</td>
      <td>36.0</td>
      <td>135.0</td>
      <td>855.0</td>
      <td>30.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>23.0</td>
      <td>38928346.0</td>
      <td>26.356116</td>
      <td>105.0</td>
      <td>113.0</td>
      <td>113.0</td>
      <td>132.0</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>99</th>
      <td>2020-04-20</td>
      <td>Albania</td>
      <td>584.0</td>
      <td>26.0</td>
      <td>327.0</td>
      <td>231.0</td>
      <td>22.0</td>
      <td>0.0</td>
      <td>13.0</td>
      <td>9.0</td>
      <td>2877797.0</td>
      <td>202.933007</td>
      <td>91.0</td>
      <td>107.0</td>
      <td>81.0</td>
      <td>117.5</td>
      <td>108.0</td>
    </tr>
    <tr>
      <th>155</th>
      <td>2020-04-20</td>
      <td>Algeria</td>
      <td>2718.0</td>
      <td>384.0</td>
      <td>1099.0</td>
      <td>1235.0</td>
      <td>89.0</td>
      <td>9.0</td>
      <td>52.0</td>
      <td>28.0</td>
      <td>43851044.0</td>
      <td>61.982561</td>
      <td>129.0</td>
      <td>137.0</td>
      <td>123.0</td>
      <td>135.5</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>205</th>
      <td>2020-04-20</td>
      <td>Andorra</td>
      <td>717.0</td>
      <td>37.0</td>
      <td>248.0</td>
      <td>432.0</td>
      <td>4.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>-10.0</td>
      <td>77265.0</td>
      <td>9279.751505</td>
      <td>98.0</td>
      <td>71.0</td>
      <td>93.0</td>
      <td>31.0</td>
      <td>176.0</td>
    </tr>
    <tr>
      <th>237</th>
      <td>2020-04-20</td>
      <td>Angola</td>
      <td>24.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>16.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>32866272.0</td>
      <td>0.730232</td>
      <td>29.5</td>
      <td>28.0</td>
      <td>27.0</td>
      <td>72.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>9011</th>
      <td>2020-04-20</td>
      <td>Vietnam</td>
      <td>268.0</td>
      <td>0.0</td>
      <td>214.0</td>
      <td>54.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.0</td>
      <td>-12.0</td>
      <td>97338579.0</td>
      <td>2.753276</td>
      <td>71.0</td>
      <td>28.0</td>
      <td>52.5</td>
      <td>26.5</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>9027</th>
      <td>2020-04-20</td>
      <td>Western Sahara</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>597339.0</td>
      <td>10.044548</td>
      <td>6.0</td>
      <td>28.0</td>
      <td>10.5</td>
      <td>72.0</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>9038</th>
      <td>2020-04-20</td>
      <td>Yemen</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>29825964.0</td>
      <td>0.033528</td>
      <td>1.0</td>
      <td>28.0</td>
      <td>3.0</td>
      <td>72.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>9072</th>
      <td>2020-04-20</td>
      <td>Zambia</td>
      <td>65.0</td>
      <td>3.0</td>
      <td>35.0</td>
      <td>27.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>18383955.0</td>
      <td>3.535692</td>
      <td>45.5</td>
      <td>71.0</td>
      <td>40.5</td>
      <td>100.0</td>
      <td>21.0</td>
    </tr>
    <tr>
      <th>9104</th>
      <td>2020-04-20</td>
      <td>Zimbabwe</td>
      <td>25.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>20.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>14862924.0</td>
      <td>1.682038</td>
      <td>31.0</td>
      <td>28.0</td>
      <td>32.0</td>
      <td>72.0</td>
      <td>13.0</td>
    </tr>
  </tbody>
</table>
<p>184 rows × 17 columns</p>
</div>



Which countries have the most confirmed cases?


```python
last_day_data.sort_values('Rank_ConfirmedAcum', ascending = False)[['Country', 'ConfirmedAcum']].reset_index(drop = True).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>ConfirmedAcum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>United States</td>
      <td>784326.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Spain</td>
      <td>200210.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Italy</td>
      <td>181228.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>France</td>
      <td>156480.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>147065.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>United Kingdom</td>
      <td>125856.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Turkey</td>
      <td>90980.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>China</td>
      <td>83817.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Iran</td>
      <td>83505.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Russia</td>
      <td>47121.0</td>
    </tr>
  </tbody>
</table>
</div>



Which countries have more confirmed cases on the last day?


```python
last_day_data.sort_values('Rank_Confirmed', ascending = False)[['Country', 'Confirmed']].reset_index(drop = True).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Confirmed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>United States</td>
      <td>25211.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>United Kingdom</td>
      <td>4684.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Turkey</td>
      <td>4674.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Russia</td>
      <td>4268.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>France</td>
      <td>2383.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Italy</td>
      <td>2256.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Brazil</td>
      <td>2089.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Canada</td>
      <td>2025.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Germany</td>
      <td>1881.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Spain</td>
      <td>1536.0</td>
    </tr>
  </tbody>
</table>
</div>



Which countries have the most active cases?


```python
last_day_data.sort_values('Rank_ActiveAcum', ascending = False)[['Country', 'ActiveAcum']].reset_index(drop = True).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>ActiveAcum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>United States</td>
      <td>669903.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>United Kingdom</td>
      <td>108860.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Italy</td>
      <td>108237.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spain</td>
      <td>98771.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>France</td>
      <td>98152.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Turkey</td>
      <td>75410.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Germany</td>
      <td>50703.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Russia</td>
      <td>43270.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Netherlands</td>
      <td>29502.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Belgium</td>
      <td>25260.0</td>
    </tr>
  </tbody>
</table>
</div>



Which countries had the most active cases on the last day?


```python
last_day_data.sort_values('Rank_Active', ascending = False)[['Country', 'Active']].reset_index(drop = True).head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>United States</td>
      <td>21786.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>United Kingdom</td>
      <td>4219.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Russia</td>
      <td>4069.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Turkey</td>
      <td>3097.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Brazil</td>
      <td>1964.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Belarus</td>
      <td>1461.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Singapore</td>
      <td>1393.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Belgium</td>
      <td>1204.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Canada</td>
      <td>1167.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Saudi Arabia</td>
      <td>1024.0</td>
    </tr>
  </tbody>
</table>
</div>



Which countries have the most active cases per million inhabitants?


```python
last_day_data.sort_values('Rank_ConfirmedAcum_Millon', ascending = False)[['Country', 'ConfirmedAcum_Millon']].reset_index(drop = True).head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>ConfirmedAcum_Millon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>San Marino</td>
      <td>13615.867496</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Holy See</td>
      <td>11235.955056</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Andorra</td>
      <td>9279.751505</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Luxembourg</td>
      <td>5683.905824</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iceland</td>
      <td>5195.710974</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Spain</td>
      <td>4282.129198</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Belgium</td>
      <td>3449.896515</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Switzerland</td>
      <td>3228.794972</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ireland</td>
      <td>3169.841706</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Italy</td>
      <td>2997.395414</td>
    </tr>
    <tr>
      <th>10</th>
      <td>France</td>
      <td>2397.297121</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Monaco</td>
      <td>2395.392692</td>
    </tr>
    <tr>
      <th>12</th>
      <td>United States</td>
      <td>2369.545977</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Liechtenstein</td>
      <td>2124.422996</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Qatar</td>
      <td>2087.778323</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Portugal</td>
      <td>2046.052310</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Netherlands</td>
      <td>1960.213067</td>
    </tr>
    <tr>
      <th>17</th>
      <td>United Kingdom</td>
      <td>1853.931291</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Germany</td>
      <td>1755.288621</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Austria</td>
      <td>1642.721097</td>
    </tr>
  </tbody>
</table>
</div>



What is the total number of cases?


```python
import locale
locale.setlocale(locale.LC_ALL, '')

total = pd.DataFrame(last_day_data[["ConfirmedAcum", "RecoveredAcum", "DeathsAcum"]].aggregate("sum")\
                     .apply(lambda value : locale.format("%d", value, grouping=True)))
total.columns = ["Total Cases"]
total.index = ['Confirmed', 'Deaths', 'Recovered']
display(total.T)

```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Confirmed</th>
      <th>Deaths</th>
      <th>Recovered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Total Cases</th>
      <td>2,472,259</td>
      <td>645,738</td>
      <td>169,986</td>
    </tr>
  </tbody>
</table>
</div>


## Saving the clean dataset

Finally I will create an Excel file with the information per country once it is clean and in perfect condition to apply some machine learning algorithms ...


```python
data_by_country.to_excel("All_data.xlsx", index = False)
```
