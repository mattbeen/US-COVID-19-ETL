# US-COVID-19-ETL
NYT Data: https://github.com/nytimes/covid-19-data/blob/master/us.csv?opt_id=oeu1602683047550r0.4929949464591361

Johns Hopkins Data: https://raw.githubusercontent.com/datasets/covid-19/master/data/time-series-19-covid-combined.csv?opt_id=oeu1602683047550r0.4929949464591361


# Run this in Python to obtain a Transformed DataFrame 

import pandas as pd

def ETL():
    us = pd.read_csv('https://raw.githubusercontent.com/nytimes/covid-19-data/master/us.csv')
    jh = pd.read_csv('https://raw.githubusercontent.com/datasets/covid-19/master/data/time-series-19-covid-combined.csv?opt_id=oeu1602683047550r0.4929949464591361')
    us['date'] = pd.to_datetime(us['date'])
    jh['Date'] = pd.to_datetime(jh['Date'])
    us_jh = jh.loc[jh['Country/Region']=='US']
    us.rename({'date': 'Date'}, axis=1,inplace=True)
    US_COVID = us.merge(us_jh, on='Date', how='left', indicator=True)
    US_COVID = US_COVID[US_COVID['_merge']=='both']
    US_COVID.drop(['Province/State','Confirmed','Deaths','_merge'],axis=1,inplace=True)
    US_COVID=US_COVID[['Date','Country/Region','cases','Recovered','deaths']]
    return US_COVID

ETL()

# DataFrame TO CSV
ETL().to_csv('US_COVID.csv')
