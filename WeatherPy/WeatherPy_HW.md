
Observations:
1) Average Temperature decreases as you get farther from the equator
2) Northern hemispher tends to have less humidity
3) Wind speed has no correlation to distance from the equator


```python
#Import Dependencies
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import requests as req
import json
import os
from citipy import citipy
import openweathermapy as ow
import datetime
from localenv import weather_api_key
```


```python
#Set DataFrame for random latitudes and longitudes
location_df=pd.DataFrame()
```


```python
#Get random latitude and longitudes. Append them to DataFram
for x in range(1000):
    rand_latitude= np.random.uniform(low=-90.000,high=90.000,size=1)
    rand_longitude=np.random.uniform(low=-180.000,high=180.000,size=1)
    random_loc=pd.DataFrame([[rand_latitude,rand_longitude]],columns=['latitude','longitude']).astype(float)
    location_df=location_df.append(random_loc)
new_location_df=location_df.reset_index()
new_location_df.head()
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
      <th>index</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>-9.629161</td>
      <td>6.963994</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>-4.004788</td>
      <td>-26.838089</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>-86.975818</td>
      <td>17.807260</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>-60.585417</td>
      <td>-23.697156</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>-6.142432</td>
      <td>120.260848</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Find nearest city to random latitude/longitude
city_list=[]
counter=0
while counter <620:
    lat=new_location_df['latitude'][counter]
    lon=new_location_df['longitude'][counter]
    city=citipy.nearest_city(lat,lon)
    city_name=city.city_name
    country_code=city.country_code
    
    if city_name not in city_list:
        city_list.append([city_name,country_code,lat,lon])
    counter +=1
```


```python
#create Data Frame for random cities
city_list_df=pd.DataFrame(city_list,columns=['city_name','country_code','latitude','longitude'])
city_list_df['city_name']
city_list_df.count()
```




    city_name       620
    country_code    620
    latitude        620
    longitude       620
    dtype: int64




```python
#set parameters for url
url = "http://api.openweathermap.org/data/2.5/weather?"
api_key= weather_api_key
api_fake_key="api_key"
url_i_noapi="{}q={}&appid={}&units=imperial".format(url,c,api_fake_key)
```


```python
#convert cities into list that we can pass through for loop
cities=city_list_df['city_name']
cities_list=list(cities.values.flatten())
len(cities_list)
```




    620




```python
#Pass list of cities through for loop to create urls
url_list=[]
count=0
for c in cities_list:
    url_id="{}q={}&appid={}&units=imperial".format(url,c,api_fake_key)
    count+=1
    print("Count:{}  City:{}  Url:{}".format(count,c,url_id))
    url_list.append(url_id)
```

    Count:1  City:luanda  Url:http://api.openweathermap.org/data/2.5/weather?q=luanda&appid=api_key&units=imperial
    Count:2  City:cabedelo  Url:http://api.openweathermap.org/data/2.5/weather?q=cabedelo&appid=api_key&units=imperial
    Count:3  City:bredasdorp  Url:http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&appid=api_key&units=imperial
    Count:4  City:chuy  Url:http://api.openweathermap.org/data/2.5/weather?q=chuy&appid=api_key&units=imperial
    Count:5  City:tanete  Url:http://api.openweathermap.org/data/2.5/weather?q=tanete&appid=api_key&units=imperial
    Count:6  City:douglas  Url:http://api.openweathermap.org/data/2.5/weather?q=douglas&appid=api_key&units=imperial
    Count:7  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:8  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:9  City:illoqqortoormiut  Url:http://api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut&appid=api_key&units=imperial
    Count:10  City:labuhan  Url:http://api.openweathermap.org/data/2.5/weather?q=labuhan&appid=api_key&units=imperial
    Count:11  City:puerto quijarro  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto quijarro&appid=api_key&units=imperial
    Count:12  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:13  City:grimshaw  Url:http://api.openweathermap.org/data/2.5/weather?q=grimshaw&appid=api_key&units=imperial
    Count:14  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:15  City:chernyshevskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=chernyshevskiy&appid=api_key&units=imperial
    Count:16  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:17  City:soyo  Url:http://api.openweathermap.org/data/2.5/weather?q=soyo&appid=api_key&units=imperial
    Count:18  City:asyut  Url:http://api.openweathermap.org/data/2.5/weather?q=asyut&appid=api_key&units=imperial
    Count:19  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:20  City:morondava  Url:http://api.openweathermap.org/data/2.5/weather?q=morondava&appid=api_key&units=imperial
    Count:21  City:mackay  Url:http://api.openweathermap.org/data/2.5/weather?q=mackay&appid=api_key&units=imperial
    Count:22  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:23  City:bargal  Url:http://api.openweathermap.org/data/2.5/weather?q=bargal&appid=api_key&units=imperial
    Count:24  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:25  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:26  City:victoria  Url:http://api.openweathermap.org/data/2.5/weather?q=victoria&appid=api_key&units=imperial
    Count:27  City:vardo  Url:http://api.openweathermap.org/data/2.5/weather?q=vardo&appid=api_key&units=imperial
    Count:28  City:hilo  Url:http://api.openweathermap.org/data/2.5/weather?q=hilo&appid=api_key&units=imperial
    Count:29  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:30  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:31  City:la ronge  Url:http://api.openweathermap.org/data/2.5/weather?q=la ronge&appid=api_key&units=imperial
    Count:32  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:33  City:los llanos de aridane  Url:http://api.openweathermap.org/data/2.5/weather?q=los llanos de aridane&appid=api_key&units=imperial
    Count:34  City:severo-kurilsk  Url:http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&appid=api_key&units=imperial
    Count:35  City:paamiut  Url:http://api.openweathermap.org/data/2.5/weather?q=paamiut&appid=api_key&units=imperial
    Count:36  City:qaanaaq  Url:http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&appid=api_key&units=imperial
    Count:37  City:luderitz  Url:http://api.openweathermap.org/data/2.5/weather?q=luderitz&appid=api_key&units=imperial
    Count:38  City:kudahuvadhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=kudahuvadhoo&appid=api_key&units=imperial
    Count:39  City:vila velha  Url:http://api.openweathermap.org/data/2.5/weather?q=vila velha&appid=api_key&units=imperial
    Count:40  City:ponta do sol  Url:http://api.openweathermap.org/data/2.5/weather?q=ponta do sol&appid=api_key&units=imperial
    Count:41  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:42  City:ilulissat  Url:http://api.openweathermap.org/data/2.5/weather?q=ilulissat&appid=api_key&units=imperial
    Count:43  City:cap malheureux  Url:http://api.openweathermap.org/data/2.5/weather?q=cap malheureux&appid=api_key&units=imperial
    Count:44  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:45  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:46  City:luderitz  Url:http://api.openweathermap.org/data/2.5/weather?q=luderitz&appid=api_key&units=imperial
    Count:47  City:lake city  Url:http://api.openweathermap.org/data/2.5/weather?q=lake city&appid=api_key&units=imperial
    Count:48  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:49  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:50  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:51  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:52  City:bredasdorp  Url:http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&appid=api_key&units=imperial
    Count:53  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:54  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:55  City:dingle  Url:http://api.openweathermap.org/data/2.5/weather?q=dingle&appid=api_key&units=imperial
    Count:56  City:verkhnevilyuysk  Url:http://api.openweathermap.org/data/2.5/weather?q=verkhnevilyuysk&appid=api_key&units=imperial
    Count:57  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:58  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:59  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:60  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:61  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:62  City:taguatinga  Url:http://api.openweathermap.org/data/2.5/weather?q=taguatinga&appid=api_key&units=imperial
    Count:63  City:groningen  Url:http://api.openweathermap.org/data/2.5/weather?q=groningen&appid=api_key&units=imperial
    Count:64  City:cidreira  Url:http://api.openweathermap.org/data/2.5/weather?q=cidreira&appid=api_key&units=imperial
    Count:65  City:iringa  Url:http://api.openweathermap.org/data/2.5/weather?q=iringa&appid=api_key&units=imperial
    Count:66  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:67  City:pitimbu  Url:http://api.openweathermap.org/data/2.5/weather?q=pitimbu&appid=api_key&units=imperial
    Count:68  City:kashi  Url:http://api.openweathermap.org/data/2.5/weather?q=kashi&appid=api_key&units=imperial
    Count:69  City:meulaboh  Url:http://api.openweathermap.org/data/2.5/weather?q=meulaboh&appid=api_key&units=imperial
    Count:70  City:khatanga  Url:http://api.openweathermap.org/data/2.5/weather?q=khatanga&appid=api_key&units=imperial
    Count:71  City:kununurra  Url:http://api.openweathermap.org/data/2.5/weather?q=kununurra&appid=api_key&units=imperial
    Count:72  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:73  City:ostrovnoy  Url:http://api.openweathermap.org/data/2.5/weather?q=ostrovnoy&appid=api_key&units=imperial
    Count:74  City:itapetinga  Url:http://api.openweathermap.org/data/2.5/weather?q=itapetinga&appid=api_key&units=imperial
    Count:75  City:sitka  Url:http://api.openweathermap.org/data/2.5/weather?q=sitka&appid=api_key&units=imperial
    Count:76  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:77  City:saskylakh  Url:http://api.openweathermap.org/data/2.5/weather?q=saskylakh&appid=api_key&units=imperial
    Count:78  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:79  City:manicore  Url:http://api.openweathermap.org/data/2.5/weather?q=manicore&appid=api_key&units=imperial
    Count:80  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:81  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:82  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:83  City:huangmei  Url:http://api.openweathermap.org/data/2.5/weather?q=huangmei&appid=api_key&units=imperial
    Count:84  City:bambous virieux  Url:http://api.openweathermap.org/data/2.5/weather?q=bambous virieux&appid=api_key&units=imperial
    Count:85  City:pisco  Url:http://api.openweathermap.org/data/2.5/weather?q=pisco&appid=api_key&units=imperial
    Count:86  City:ndele  Url:http://api.openweathermap.org/data/2.5/weather?q=ndele&appid=api_key&units=imperial
    Count:87  City:lata  Url:http://api.openweathermap.org/data/2.5/weather?q=lata&appid=api_key&units=imperial
    Count:88  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:89  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:90  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:91  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:92  City:iberia  Url:http://api.openweathermap.org/data/2.5/weather?q=iberia&appid=api_key&units=imperial
    Count:93  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:94  City:ahuimanu  Url:http://api.openweathermap.org/data/2.5/weather?q=ahuimanu&appid=api_key&units=imperial
    Count:95  City:namatanai  Url:http://api.openweathermap.org/data/2.5/weather?q=namatanai&appid=api_key&units=imperial
    Count:96  City:lompoc  Url:http://api.openweathermap.org/data/2.5/weather?q=lompoc&appid=api_key&units=imperial
    Count:97  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:98  City:flinders  Url:http://api.openweathermap.org/data/2.5/weather?q=flinders&appid=api_key&units=imperial
    Count:99  City:san patricio  Url:http://api.openweathermap.org/data/2.5/weather?q=san patricio&appid=api_key&units=imperial
    Count:100  City:katsuura  Url:http://api.openweathermap.org/data/2.5/weather?q=katsuura&appid=api_key&units=imperial
    Count:101  City:port alfred  Url:http://api.openweathermap.org/data/2.5/weather?q=port alfred&appid=api_key&units=imperial
    Count:102  City:senneterre  Url:http://api.openweathermap.org/data/2.5/weather?q=senneterre&appid=api_key&units=imperial
    Count:103  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:104  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:105  City:saint george  Url:http://api.openweathermap.org/data/2.5/weather?q=saint george&appid=api_key&units=imperial
    Count:106  City:labrea  Url:http://api.openweathermap.org/data/2.5/weather?q=labrea&appid=api_key&units=imperial
    Count:107  City:saskylakh  Url:http://api.openweathermap.org/data/2.5/weather?q=saskylakh&appid=api_key&units=imperial
    Count:108  City:hithadhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&appid=api_key&units=imperial
    Count:109  City:yinchuan  Url:http://api.openweathermap.org/data/2.5/weather?q=yinchuan&appid=api_key&units=imperial
    Count:110  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:111  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:112  City:tsihombe  Url:http://api.openweathermap.org/data/2.5/weather?q=tsihombe&appid=api_key&units=imperial
    Count:113  City:jalu  Url:http://api.openweathermap.org/data/2.5/weather?q=jalu&appid=api_key&units=imperial
    Count:114  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:115  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:116  City:solnechnyy  Url:http://api.openweathermap.org/data/2.5/weather?q=solnechnyy&appid=api_key&units=imperial
    Count:117  City:namibe  Url:http://api.openweathermap.org/data/2.5/weather?q=namibe&appid=api_key&units=imperial
    Count:118  City:ribeira grande  Url:http://api.openweathermap.org/data/2.5/weather?q=ribeira grande&appid=api_key&units=imperial
    Count:119  City:narsaq  Url:http://api.openweathermap.org/data/2.5/weather?q=narsaq&appid=api_key&units=imperial
    Count:120  City:sabang  Url:http://api.openweathermap.org/data/2.5/weather?q=sabang&appid=api_key&units=imperial
    Count:121  City:taonan  Url:http://api.openweathermap.org/data/2.5/weather?q=taonan&appid=api_key&units=imperial
    Count:122  City:balykshi  Url:http://api.openweathermap.org/data/2.5/weather?q=balykshi&appid=api_key&units=imperial
    Count:123  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:124  City:sinnamary  Url:http://api.openweathermap.org/data/2.5/weather?q=sinnamary&appid=api_key&units=imperial
    Count:125  City:aksu  Url:http://api.openweathermap.org/data/2.5/weather?q=aksu&appid=api_key&units=imperial
    Count:126  City:vila velha  Url:http://api.openweathermap.org/data/2.5/weather?q=vila velha&appid=api_key&units=imperial
    Count:127  City:georgetown  Url:http://api.openweathermap.org/data/2.5/weather?q=georgetown&appid=api_key&units=imperial
    Count:128  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:129  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:130  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:131  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:132  City:beloha  Url:http://api.openweathermap.org/data/2.5/weather?q=beloha&appid=api_key&units=imperial
    Count:133  City:saskylakh  Url:http://api.openweathermap.org/data/2.5/weather?q=saskylakh&appid=api_key&units=imperial
    Count:134  City:raga  Url:http://api.openweathermap.org/data/2.5/weather?q=raga&appid=api_key&units=imperial
    Count:135  City:lazaro cardenas  Url:http://api.openweathermap.org/data/2.5/weather?q=lazaro cardenas&appid=api_key&units=imperial
    Count:136  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:137  City:berlevag  Url:http://api.openweathermap.org/data/2.5/weather?q=berlevag&appid=api_key&units=imperial
    Count:138  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:139  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:140  City:valenca do piaui  Url:http://api.openweathermap.org/data/2.5/weather?q=valenca do piaui&appid=api_key&units=imperial
    Count:141  City:burica  Url:http://api.openweathermap.org/data/2.5/weather?q=burica&appid=api_key&units=imperial
    Count:142  City:talara  Url:http://api.openweathermap.org/data/2.5/weather?q=talara&appid=api_key&units=imperial
    Count:143  City:castro  Url:http://api.openweathermap.org/data/2.5/weather?q=castro&appid=api_key&units=imperial
    Count:144  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:145  City:saskylakh  Url:http://api.openweathermap.org/data/2.5/weather?q=saskylakh&appid=api_key&units=imperial
    Count:146  City:khatanga  Url:http://api.openweathermap.org/data/2.5/weather?q=khatanga&appid=api_key&units=imperial
    Count:147  City:torbay  Url:http://api.openweathermap.org/data/2.5/weather?q=torbay&appid=api_key&units=imperial
    Count:148  City:nabire  Url:http://api.openweathermap.org/data/2.5/weather?q=nabire&appid=api_key&units=imperial
    Count:149  City:kruisfontein  Url:http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&appid=api_key&units=imperial
    Count:150  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:151  City:saint-francois  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-francois&appid=api_key&units=imperial
    Count:152  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:153  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:154  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:155  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:156  City:stokmarknes  Url:http://api.openweathermap.org/data/2.5/weather?q=stokmarknes&appid=api_key&units=imperial
    Count:157  City:kruisfontein  Url:http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&appid=api_key&units=imperial
    Count:158  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:159  City:sambava  Url:http://api.openweathermap.org/data/2.5/weather?q=sambava&appid=api_key&units=imperial
    Count:160  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:161  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:162  City:nome  Url:http://api.openweathermap.org/data/2.5/weather?q=nome&appid=api_key&units=imperial
    Count:163  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:164  City:bonthe  Url:http://api.openweathermap.org/data/2.5/weather?q=bonthe&appid=api_key&units=imperial
    Count:165  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:166  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:167  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:168  City:marcona  Url:http://api.openweathermap.org/data/2.5/weather?q=marcona&appid=api_key&units=imperial
    Count:169  City:airai  Url:http://api.openweathermap.org/data/2.5/weather?q=airai&appid=api_key&units=imperial
    Count:170  City:chuy  Url:http://api.openweathermap.org/data/2.5/weather?q=chuy&appid=api_key&units=imperial
    Count:171  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:172  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:173  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:174  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:175  City:kirakira  Url:http://api.openweathermap.org/data/2.5/weather?q=kirakira&appid=api_key&units=imperial
    Count:176  City:pemangkat  Url:http://api.openweathermap.org/data/2.5/weather?q=pemangkat&appid=api_key&units=imperial
    Count:177  City:itoman  Url:http://api.openweathermap.org/data/2.5/weather?q=itoman&appid=api_key&units=imperial
    Count:178  City:iskateley  Url:http://api.openweathermap.org/data/2.5/weather?q=iskateley&appid=api_key&units=imperial
    Count:179  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:180  City:laizhou  Url:http://api.openweathermap.org/data/2.5/weather?q=laizhou&appid=api_key&units=imperial
    Count:181  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:182  City:narsaq  Url:http://api.openweathermap.org/data/2.5/weather?q=narsaq&appid=api_key&units=imperial
    Count:183  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:184  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:185  City:groa  Url:http://api.openweathermap.org/data/2.5/weather?q=groa&appid=api_key&units=imperial
    Count:186  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:187  City:port alfred  Url:http://api.openweathermap.org/data/2.5/weather?q=port alfred&appid=api_key&units=imperial
    Count:188  City:mayya  Url:http://api.openweathermap.org/data/2.5/weather?q=mayya&appid=api_key&units=imperial
    Count:189  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:190  City:zhigansk  Url:http://api.openweathermap.org/data/2.5/weather?q=zhigansk&appid=api_key&units=imperial
    Count:191  City:gondanglegi  Url:http://api.openweathermap.org/data/2.5/weather?q=gondanglegi&appid=api_key&units=imperial
    Count:192  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:193  City:general roca  Url:http://api.openweathermap.org/data/2.5/weather?q=general roca&appid=api_key&units=imperial
    Count:194  City:norman wells  Url:http://api.openweathermap.org/data/2.5/weather?q=norman wells&appid=api_key&units=imperial
    Count:195  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:196  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:197  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:198  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:199  City:pisco  Url:http://api.openweathermap.org/data/2.5/weather?q=pisco&appid=api_key&units=imperial
    Count:200  City:iqaluit  Url:http://api.openweathermap.org/data/2.5/weather?q=iqaluit&appid=api_key&units=imperial
    Count:201  City:new norfolk  Url:http://api.openweathermap.org/data/2.5/weather?q=new norfolk&appid=api_key&units=imperial
    Count:202  City:luanda  Url:http://api.openweathermap.org/data/2.5/weather?q=luanda&appid=api_key&units=imperial
    Count:203  City:tsihombe  Url:http://api.openweathermap.org/data/2.5/weather?q=tsihombe&appid=api_key&units=imperial
    Count:204  City:marzuq  Url:http://api.openweathermap.org/data/2.5/weather?q=marzuq&appid=api_key&units=imperial
    Count:205  City:pervomayskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=pervomayskiy&appid=api_key&units=imperial
    Count:206  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:207  City:raudeberg  Url:http://api.openweathermap.org/data/2.5/weather?q=raudeberg&appid=api_key&units=imperial
    Count:208  City:kungurtug  Url:http://api.openweathermap.org/data/2.5/weather?q=kungurtug&appid=api_key&units=imperial
    Count:209  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:210  City:mino  Url:http://api.openweathermap.org/data/2.5/weather?q=mino&appid=api_key&units=imperial
    Count:211  City:ust-kamchatsk  Url:http://api.openweathermap.org/data/2.5/weather?q=ust-kamchatsk&appid=api_key&units=imperial
    Count:212  City:vardo  Url:http://api.openweathermap.org/data/2.5/weather?q=vardo&appid=api_key&units=imperial
    Count:213  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:214  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:215  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:216  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:217  City:porto novo  Url:http://api.openweathermap.org/data/2.5/weather?q=porto novo&appid=api_key&units=imperial
    Count:218  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:219  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:220  City:dingle  Url:http://api.openweathermap.org/data/2.5/weather?q=dingle&appid=api_key&units=imperial
    Count:221  City:san andres  Url:http://api.openweathermap.org/data/2.5/weather?q=san andres&appid=api_key&units=imperial
    Count:222  City:caravelas  Url:http://api.openweathermap.org/data/2.5/weather?q=caravelas&appid=api_key&units=imperial
    Count:223  City:praia da vitoria  Url:http://api.openweathermap.org/data/2.5/weather?q=praia da vitoria&appid=api_key&units=imperial
    Count:224  City:lebu  Url:http://api.openweathermap.org/data/2.5/weather?q=lebu&appid=api_key&units=imperial
    Count:225  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:226  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:227  City:byron bay  Url:http://api.openweathermap.org/data/2.5/weather?q=byron bay&appid=api_key&units=imperial
    Count:228  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:229  City:te anau  Url:http://api.openweathermap.org/data/2.5/weather?q=te anau&appid=api_key&units=imperial
    Count:230  City:samusu  Url:http://api.openweathermap.org/data/2.5/weather?q=samusu&appid=api_key&units=imperial
    Count:231  City:illoqqortoormiut  Url:http://api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut&appid=api_key&units=imperial
    Count:232  City:college  Url:http://api.openweathermap.org/data/2.5/weather?q=college&appid=api_key&units=imperial
    Count:233  City:xining  Url:http://api.openweathermap.org/data/2.5/weather?q=xining&appid=api_key&units=imperial
    Count:234  City:qaanaaq  Url:http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&appid=api_key&units=imperial
    Count:235  City:kruisfontein  Url:http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&appid=api_key&units=imperial
    Count:236  City:carnarvon  Url:http://api.openweathermap.org/data/2.5/weather?q=carnarvon&appid=api_key&units=imperial
    Count:237  City:beringovskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=beringovskiy&appid=api_key&units=imperial
    Count:238  City:maragogi  Url:http://api.openweathermap.org/data/2.5/weather?q=maragogi&appid=api_key&units=imperial
    Count:239  City:hobyo  Url:http://api.openweathermap.org/data/2.5/weather?q=hobyo&appid=api_key&units=imperial
    Count:240  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:241  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:242  City:mahebourg  Url:http://api.openweathermap.org/data/2.5/weather?q=mahebourg&appid=api_key&units=imperial
    Count:243  City:bredasdorp  Url:http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&appid=api_key&units=imperial
    Count:244  City:muravlenko  Url:http://api.openweathermap.org/data/2.5/weather?q=muravlenko&appid=api_key&units=imperial
    Count:245  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:246  City:cayenne  Url:http://api.openweathermap.org/data/2.5/weather?q=cayenne&appid=api_key&units=imperial
    Count:247  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:248  City:longyearbyen  Url:http://api.openweathermap.org/data/2.5/weather?q=longyearbyen&appid=api_key&units=imperial
    Count:249  City:san cristobal  Url:http://api.openweathermap.org/data/2.5/weather?q=san cristobal&appid=api_key&units=imperial
    Count:250  City:victoria  Url:http://api.openweathermap.org/data/2.5/weather?q=victoria&appid=api_key&units=imperial
    Count:251  City:jalu  Url:http://api.openweathermap.org/data/2.5/weather?q=jalu&appid=api_key&units=imperial
    Count:252  City:ponta do sol  Url:http://api.openweathermap.org/data/2.5/weather?q=ponta do sol&appid=api_key&units=imperial
    Count:253  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:254  City:alotau  Url:http://api.openweathermap.org/data/2.5/weather?q=alotau&appid=api_key&units=imperial
    Count:255  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:256  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:257  City:bengkulu  Url:http://api.openweathermap.org/data/2.5/weather?q=bengkulu&appid=api_key&units=imperial
    Count:258  City:belyy yar  Url:http://api.openweathermap.org/data/2.5/weather?q=belyy yar&appid=api_key&units=imperial
    Count:259  City:upernavik  Url:http://api.openweathermap.org/data/2.5/weather?q=upernavik&appid=api_key&units=imperial
    Count:260  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:261  City:victoria  Url:http://api.openweathermap.org/data/2.5/weather?q=victoria&appid=api_key&units=imperial
    Count:262  City:skalistyy  Url:http://api.openweathermap.org/data/2.5/weather?q=skalistyy&appid=api_key&units=imperial
    Count:263  City:catia la mar  Url:http://api.openweathermap.org/data/2.5/weather?q=catia la mar&appid=api_key&units=imperial
    Count:264  City:grand river south east  Url:http://api.openweathermap.org/data/2.5/weather?q=grand river south east&appid=api_key&units=imperial
    Count:265  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:266  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:267  City:pevek  Url:http://api.openweathermap.org/data/2.5/weather?q=pevek&appid=api_key&units=imperial
    Count:268  City:sungai besar  Url:http://api.openweathermap.org/data/2.5/weather?q=sungai besar&appid=api_key&units=imperial
    Count:269  City:puerto narino  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto narino&appid=api_key&units=imperial
    Count:270  City:brasilia  Url:http://api.openweathermap.org/data/2.5/weather?q=brasilia&appid=api_key&units=imperial
    Count:271  City:buraydah  Url:http://api.openweathermap.org/data/2.5/weather?q=buraydah&appid=api_key&units=imperial
    Count:272  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:273  City:tabou  Url:http://api.openweathermap.org/data/2.5/weather?q=tabou&appid=api_key&units=imperial
    Count:274  City:la ronge  Url:http://api.openweathermap.org/data/2.5/weather?q=la ronge&appid=api_key&units=imperial
    Count:275  City:tuatapere  Url:http://api.openweathermap.org/data/2.5/weather?q=tuatapere&appid=api_key&units=imperial
    Count:276  City:butaritari  Url:http://api.openweathermap.org/data/2.5/weather?q=butaritari&appid=api_key&units=imperial
    Count:277  City:kruisfontein  Url:http://api.openweathermap.org/data/2.5/weather?q=kruisfontein&appid=api_key&units=imperial
    Count:278  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:279  City:oktyabrskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=oktyabrskiy&appid=api_key&units=imperial
    Count:280  City:wattegama  Url:http://api.openweathermap.org/data/2.5/weather?q=wattegama&appid=api_key&units=imperial
    Count:281  City:severo-kurilsk  Url:http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&appid=api_key&units=imperial
    Count:282  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:283  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:284  City:sayyan  Url:http://api.openweathermap.org/data/2.5/weather?q=sayyan&appid=api_key&units=imperial
    Count:285  City:yumen  Url:http://api.openweathermap.org/data/2.5/weather?q=yumen&appid=api_key&units=imperial
    Count:286  City:khani  Url:http://api.openweathermap.org/data/2.5/weather?q=khani&appid=api_key&units=imperial
    Count:287  City:kawalu  Url:http://api.openweathermap.org/data/2.5/weather?q=kawalu&appid=api_key&units=imperial
    Count:288  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:289  City:tumannyy  Url:http://api.openweathermap.org/data/2.5/weather?q=tumannyy&appid=api_key&units=imperial
    Count:290  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:291  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:292  City:kaitangata  Url:http://api.openweathermap.org/data/2.5/weather?q=kaitangata&appid=api_key&units=imperial
    Count:293  City:avarua  Url:http://api.openweathermap.org/data/2.5/weather?q=avarua&appid=api_key&units=imperial
    Count:294  City:chimore  Url:http://api.openweathermap.org/data/2.5/weather?q=chimore&appid=api_key&units=imperial
    Count:295  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:296  City:kahului  Url:http://api.openweathermap.org/data/2.5/weather?q=kahului&appid=api_key&units=imperial
    Count:297  City:torbay  Url:http://api.openweathermap.org/data/2.5/weather?q=torbay&appid=api_key&units=imperial
    Count:298  City:avarua  Url:http://api.openweathermap.org/data/2.5/weather?q=avarua&appid=api_key&units=imperial
    Count:299  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:300  City:togur  Url:http://api.openweathermap.org/data/2.5/weather?q=togur&appid=api_key&units=imperial
    Count:301  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:302  City:talesh  Url:http://api.openweathermap.org/data/2.5/weather?q=talesh&appid=api_key&units=imperial
    Count:303  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:304  City:pamekasan  Url:http://api.openweathermap.org/data/2.5/weather?q=pamekasan&appid=api_key&units=imperial
    Count:305  City:hithadhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&appid=api_key&units=imperial
    Count:306  City:bredasdorp  Url:http://api.openweathermap.org/data/2.5/weather?q=bredasdorp&appid=api_key&units=imperial
    Count:307  City:norman wells  Url:http://api.openweathermap.org/data/2.5/weather?q=norman wells&appid=api_key&units=imperial
    Count:308  City:aklavik  Url:http://api.openweathermap.org/data/2.5/weather?q=aklavik&appid=api_key&units=imperial
    Count:309  City:kimbe  Url:http://api.openweathermap.org/data/2.5/weather?q=kimbe&appid=api_key&units=imperial
    Count:310  City:tungor  Url:http://api.openweathermap.org/data/2.5/weather?q=tungor&appid=api_key&units=imperial
    Count:311  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:312  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:313  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:314  City:labuan  Url:http://api.openweathermap.org/data/2.5/weather?q=labuan&appid=api_key&units=imperial
    Count:315  City:havelock  Url:http://api.openweathermap.org/data/2.5/weather?q=havelock&appid=api_key&units=imperial
    Count:316  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:317  City:sitka  Url:http://api.openweathermap.org/data/2.5/weather?q=sitka&appid=api_key&units=imperial
    Count:318  City:kamenice  Url:http://api.openweathermap.org/data/2.5/weather?q=kamenice&appid=api_key&units=imperial
    Count:319  City:goderich  Url:http://api.openweathermap.org/data/2.5/weather?q=goderich&appid=api_key&units=imperial
    Count:320  City:illoqqortoormiut  Url:http://api.openweathermap.org/data/2.5/weather?q=illoqqortoormiut&appid=api_key&units=imperial
    Count:321  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:322  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:323  City:chokurdakh  Url:http://api.openweathermap.org/data/2.5/weather?q=chokurdakh&appid=api_key&units=imperial
    Count:324  City:manhattan  Url:http://api.openweathermap.org/data/2.5/weather?q=manhattan&appid=api_key&units=imperial
    Count:325  City:ijaki  Url:http://api.openweathermap.org/data/2.5/weather?q=ijaki&appid=api_key&units=imperial
    Count:326  City:kapaa  Url:http://api.openweathermap.org/data/2.5/weather?q=kapaa&appid=api_key&units=imperial
    Count:327  City:yanchukan  Url:http://api.openweathermap.org/data/2.5/weather?q=yanchukan&appid=api_key&units=imperial
    Count:328  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:329  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:330  City:kaolinovo  Url:http://api.openweathermap.org/data/2.5/weather?q=kaolinovo&appid=api_key&units=imperial
    Count:331  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:332  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:333  City:george town  Url:http://api.openweathermap.org/data/2.5/weather?q=george town&appid=api_key&units=imperial
    Count:334  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:335  City:port elizabeth  Url:http://api.openweathermap.org/data/2.5/weather?q=port elizabeth&appid=api_key&units=imperial
    Count:336  City:kununurra  Url:http://api.openweathermap.org/data/2.5/weather?q=kununurra&appid=api_key&units=imperial
    Count:337  City:atraulia  Url:http://api.openweathermap.org/data/2.5/weather?q=atraulia&appid=api_key&units=imperial
    Count:338  City:shitanjing  Url:http://api.openweathermap.org/data/2.5/weather?q=shitanjing&appid=api_key&units=imperial
    Count:339  City:mount isa  Url:http://api.openweathermap.org/data/2.5/weather?q=mount isa&appid=api_key&units=imperial
    Count:340  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:341  City:vardo  Url:http://api.openweathermap.org/data/2.5/weather?q=vardo&appid=api_key&units=imperial
    Count:342  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:343  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:344  City:bhaktapur  Url:http://api.openweathermap.org/data/2.5/weather?q=bhaktapur&appid=api_key&units=imperial
    Count:345  City:machachi  Url:http://api.openweathermap.org/data/2.5/weather?q=machachi&appid=api_key&units=imperial
    Count:346  City:honiara  Url:http://api.openweathermap.org/data/2.5/weather?q=honiara&appid=api_key&units=imperial
    Count:347  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:348  City:fortuna  Url:http://api.openweathermap.org/data/2.5/weather?q=fortuna&appid=api_key&units=imperial
    Count:349  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:350  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:351  City:haimen  Url:http://api.openweathermap.org/data/2.5/weather?q=haimen&appid=api_key&units=imperial
    Count:352  City:grindavik  Url:http://api.openweathermap.org/data/2.5/weather?q=grindavik&appid=api_key&units=imperial
    Count:353  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:354  City:viedma  Url:http://api.openweathermap.org/data/2.5/weather?q=viedma&appid=api_key&units=imperial
    Count:355  City:longyearbyen  Url:http://api.openweathermap.org/data/2.5/weather?q=longyearbyen&appid=api_key&units=imperial
    Count:356  City:tocopilla  Url:http://api.openweathermap.org/data/2.5/weather?q=tocopilla&appid=api_key&units=imperial
    Count:357  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:358  City:lasa  Url:http://api.openweathermap.org/data/2.5/weather?q=lasa&appid=api_key&units=imperial
    Count:359  City:mount pleasant  Url:http://api.openweathermap.org/data/2.5/weather?q=mount pleasant&appid=api_key&units=imperial
    Count:360  City:provideniya  Url:http://api.openweathermap.org/data/2.5/weather?q=provideniya&appid=api_key&units=imperial
    Count:361  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:362  City:codrington  Url:http://api.openweathermap.org/data/2.5/weather?q=codrington&appid=api_key&units=imperial
    Count:363  City:bosaso  Url:http://api.openweathermap.org/data/2.5/weather?q=bosaso&appid=api_key&units=imperial
    Count:364  City:prince rupert  Url:http://api.openweathermap.org/data/2.5/weather?q=prince rupert&appid=api_key&units=imperial
    Count:365  City:noumea  Url:http://api.openweathermap.org/data/2.5/weather?q=noumea&appid=api_key&units=imperial
    Count:366  City:bethel  Url:http://api.openweathermap.org/data/2.5/weather?q=bethel&appid=api_key&units=imperial
    Count:367  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:368  City:mahebourg  Url:http://api.openweathermap.org/data/2.5/weather?q=mahebourg&appid=api_key&units=imperial
    Count:369  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:370  City:vardo  Url:http://api.openweathermap.org/data/2.5/weather?q=vardo&appid=api_key&units=imperial
    Count:371  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:372  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:373  City:comodoro rivadavia  Url:http://api.openweathermap.org/data/2.5/weather?q=comodoro rivadavia&appid=api_key&units=imperial
    Count:374  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:375  City:puerto del rosario  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto del rosario&appid=api_key&units=imperial
    Count:376  City:attawapiskat  Url:http://api.openweathermap.org/data/2.5/weather?q=attawapiskat&appid=api_key&units=imperial
    Count:377  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:378  City:tuy hoa  Url:http://api.openweathermap.org/data/2.5/weather?q=tuy hoa&appid=api_key&units=imperial
    Count:379  City:grand river south east  Url:http://api.openweathermap.org/data/2.5/weather?q=grand river south east&appid=api_key&units=imperial
    Count:380  City:pisco  Url:http://api.openweathermap.org/data/2.5/weather?q=pisco&appid=api_key&units=imperial
    Count:381  City:paamiut  Url:http://api.openweathermap.org/data/2.5/weather?q=paamiut&appid=api_key&units=imperial
    Count:382  City:belushya guba  Url:http://api.openweathermap.org/data/2.5/weather?q=belushya guba&appid=api_key&units=imperial
    Count:383  City:keti bandar  Url:http://api.openweathermap.org/data/2.5/weather?q=keti bandar&appid=api_key&units=imperial
    Count:384  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:385  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:386  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:387  City:ancona  Url:http://api.openweathermap.org/data/2.5/weather?q=ancona&appid=api_key&units=imperial
    Count:388  City:mullaitivu  Url:http://api.openweathermap.org/data/2.5/weather?q=mullaitivu&appid=api_key&units=imperial
    Count:389  City:cherskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=cherskiy&appid=api_key&units=imperial
    Count:390  City:kapaa  Url:http://api.openweathermap.org/data/2.5/weather?q=kapaa&appid=api_key&units=imperial
    Count:391  City:grand-couronne  Url:http://api.openweathermap.org/data/2.5/weather?q=grand-couronne&appid=api_key&units=imperial
    Count:392  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:393  City:nuevo progreso  Url:http://api.openweathermap.org/data/2.5/weather?q=nuevo progreso&appid=api_key&units=imperial
    Count:394  City:xining  Url:http://api.openweathermap.org/data/2.5/weather?q=xining&appid=api_key&units=imperial
    Count:395  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:396  City:amderma  Url:http://api.openweathermap.org/data/2.5/weather?q=amderma&appid=api_key&units=imperial
    Count:397  City:alexandria  Url:http://api.openweathermap.org/data/2.5/weather?q=alexandria&appid=api_key&units=imperial
    Count:398  City:shahpura  Url:http://api.openweathermap.org/data/2.5/weather?q=shahpura&appid=api_key&units=imperial
    Count:399  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:400  City:kapaa  Url:http://api.openweathermap.org/data/2.5/weather?q=kapaa&appid=api_key&units=imperial
    Count:401  City:honningsvag  Url:http://api.openweathermap.org/data/2.5/weather?q=honningsvag&appid=api_key&units=imperial
    Count:402  City:kavaratti  Url:http://api.openweathermap.org/data/2.5/weather?q=kavaratti&appid=api_key&units=imperial
    Count:403  City:constitucion  Url:http://api.openweathermap.org/data/2.5/weather?q=constitucion&appid=api_key&units=imperial
    Count:404  City:faya  Url:http://api.openweathermap.org/data/2.5/weather?q=faya&appid=api_key&units=imperial
    Count:405  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:406  City:upernavik  Url:http://api.openweathermap.org/data/2.5/weather?q=upernavik&appid=api_key&units=imperial
    Count:407  City:port keats  Url:http://api.openweathermap.org/data/2.5/weather?q=port keats&appid=api_key&units=imperial
    Count:408  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:409  City:halalo  Url:http://api.openweathermap.org/data/2.5/weather?q=halalo&appid=api_key&units=imperial
    Count:410  City:albany  Url:http://api.openweathermap.org/data/2.5/weather?q=albany&appid=api_key&units=imperial
    Count:411  City:laguna  Url:http://api.openweathermap.org/data/2.5/weather?q=laguna&appid=api_key&units=imperial
    Count:412  City:padang  Url:http://api.openweathermap.org/data/2.5/weather?q=padang&appid=api_key&units=imperial
    Count:413  City:zalantun  Url:http://api.openweathermap.org/data/2.5/weather?q=zalantun&appid=api_key&units=imperial
    Count:414  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:415  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:416  City:komsomolskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=komsomolskiy&appid=api_key&units=imperial
    Count:417  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:418  City:qaanaaq  Url:http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&appid=api_key&units=imperial
    Count:419  City:saleilua  Url:http://api.openweathermap.org/data/2.5/weather?q=saleilua&appid=api_key&units=imperial
    Count:420  City:iskateley  Url:http://api.openweathermap.org/data/2.5/weather?q=iskateley&appid=api_key&units=imperial
    Count:421  City:kaitangata  Url:http://api.openweathermap.org/data/2.5/weather?q=kaitangata&appid=api_key&units=imperial
    Count:422  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:423  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:424  City:dikson  Url:http://api.openweathermap.org/data/2.5/weather?q=dikson&appid=api_key&units=imperial
    Count:425  City:sobolevo  Url:http://api.openweathermap.org/data/2.5/weather?q=sobolevo&appid=api_key&units=imperial
    Count:426  City:karpogory  Url:http://api.openweathermap.org/data/2.5/weather?q=karpogory&appid=api_key&units=imperial
    Count:427  City:fengrun  Url:http://api.openweathermap.org/data/2.5/weather?q=fengrun&appid=api_key&units=imperial
    Count:428  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:429  City:ruatoria  Url:http://api.openweathermap.org/data/2.5/weather?q=ruatoria&appid=api_key&units=imperial
    Count:430  City:tilichiki  Url:http://api.openweathermap.org/data/2.5/weather?q=tilichiki&appid=api_key&units=imperial
    Count:431  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:432  City:qaanaaq  Url:http://api.openweathermap.org/data/2.5/weather?q=qaanaaq&appid=api_key&units=imperial
    Count:433  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:434  City:tumannyy  Url:http://api.openweathermap.org/data/2.5/weather?q=tumannyy&appid=api_key&units=imperial
    Count:435  City:tuktoyaktuk  Url:http://api.openweathermap.org/data/2.5/weather?q=tuktoyaktuk&appid=api_key&units=imperial
    Count:436  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:437  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:438  City:shenzhen  Url:http://api.openweathermap.org/data/2.5/weather?q=shenzhen&appid=api_key&units=imperial
    Count:439  City:jatai  Url:http://api.openweathermap.org/data/2.5/weather?q=jatai&appid=api_key&units=imperial
    Count:440  City:mugumu  Url:http://api.openweathermap.org/data/2.5/weather?q=mugumu&appid=api_key&units=imperial
    Count:441  City:cockburn town  Url:http://api.openweathermap.org/data/2.5/weather?q=cockburn town&appid=api_key&units=imperial
    Count:442  City:castro  Url:http://api.openweathermap.org/data/2.5/weather?q=castro&appid=api_key&units=imperial
    Count:443  City:cockburn town  Url:http://api.openweathermap.org/data/2.5/weather?q=cockburn town&appid=api_key&units=imperial
    Count:444  City:aklavik  Url:http://api.openweathermap.org/data/2.5/weather?q=aklavik&appid=api_key&units=imperial
    Count:445  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:446  City:mecca  Url:http://api.openweathermap.org/data/2.5/weather?q=mecca&appid=api_key&units=imperial
    Count:447  City:bogatic  Url:http://api.openweathermap.org/data/2.5/weather?q=bogatic&appid=api_key&units=imperial
    Count:448  City:cayenne  Url:http://api.openweathermap.org/data/2.5/weather?q=cayenne&appid=api_key&units=imperial
    Count:449  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:450  City:east london  Url:http://api.openweathermap.org/data/2.5/weather?q=east london&appid=api_key&units=imperial
    Count:451  City:sitka  Url:http://api.openweathermap.org/data/2.5/weather?q=sitka&appid=api_key&units=imperial
    Count:452  City:cidreira  Url:http://api.openweathermap.org/data/2.5/weather?q=cidreira&appid=api_key&units=imperial
    Count:453  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:454  City:new norfolk  Url:http://api.openweathermap.org/data/2.5/weather?q=new norfolk&appid=api_key&units=imperial
    Count:455  City:cidreira  Url:http://api.openweathermap.org/data/2.5/weather?q=cidreira&appid=api_key&units=imperial
    Count:456  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:457  City:butaritari  Url:http://api.openweathermap.org/data/2.5/weather?q=butaritari&appid=api_key&units=imperial
    Count:458  City:kikwit  Url:http://api.openweathermap.org/data/2.5/weather?q=kikwit&appid=api_key&units=imperial
    Count:459  City:saldanha  Url:http://api.openweathermap.org/data/2.5/weather?q=saldanha&appid=api_key&units=imperial
    Count:460  City:saint-philippe  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-philippe&appid=api_key&units=imperial
    Count:461  City:jalu  Url:http://api.openweathermap.org/data/2.5/weather?q=jalu&appid=api_key&units=imperial
    Count:462  City:isfana  Url:http://api.openweathermap.org/data/2.5/weather?q=isfana&appid=api_key&units=imperial
    Count:463  City:shiyan  Url:http://api.openweathermap.org/data/2.5/weather?q=shiyan&appid=api_key&units=imperial
    Count:464  City:margate  Url:http://api.openweathermap.org/data/2.5/weather?q=margate&appid=api_key&units=imperial
    Count:465  City:saint-augustin  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-augustin&appid=api_key&units=imperial
    Count:466  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:467  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:468  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:469  City:sentyabrskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=sentyabrskiy&appid=api_key&units=imperial
    Count:470  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:471  City:sur  Url:http://api.openweathermap.org/data/2.5/weather?q=sur&appid=api_key&units=imperial
    Count:472  City:bowen  Url:http://api.openweathermap.org/data/2.5/weather?q=bowen&appid=api_key&units=imperial
    Count:473  City:maldonado  Url:http://api.openweathermap.org/data/2.5/weather?q=maldonado&appid=api_key&units=imperial
    Count:474  City:kawalu  Url:http://api.openweathermap.org/data/2.5/weather?q=kawalu&appid=api_key&units=imperial
    Count:475  City:clyde river  Url:http://api.openweathermap.org/data/2.5/weather?q=clyde river&appid=api_key&units=imperial
    Count:476  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:477  City:severo-kurilsk  Url:http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&appid=api_key&units=imperial
    Count:478  City:marawi  Url:http://api.openweathermap.org/data/2.5/weather?q=marawi&appid=api_key&units=imperial
    Count:479  City:manta  Url:http://api.openweathermap.org/data/2.5/weather?q=manta&appid=api_key&units=imperial
    Count:480  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:481  City:esperance  Url:http://api.openweathermap.org/data/2.5/weather?q=esperance&appid=api_key&units=imperial
    Count:482  City:mulki  Url:http://api.openweathermap.org/data/2.5/weather?q=mulki&appid=api_key&units=imperial
    Count:483  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:484  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:485  City:kieta  Url:http://api.openweathermap.org/data/2.5/weather?q=kieta&appid=api_key&units=imperial
    Count:486  City:tabou  Url:http://api.openweathermap.org/data/2.5/weather?q=tabou&appid=api_key&units=imperial
    Count:487  City:korla  Url:http://api.openweathermap.org/data/2.5/weather?q=korla&appid=api_key&units=imperial
    Count:488  City:ondorhaan  Url:http://api.openweathermap.org/data/2.5/weather?q=ondorhaan&appid=api_key&units=imperial
    Count:489  City:caravelas  Url:http://api.openweathermap.org/data/2.5/weather?q=caravelas&appid=api_key&units=imperial
    Count:490  City:haguenau  Url:http://api.openweathermap.org/data/2.5/weather?q=haguenau&appid=api_key&units=imperial
    Count:491  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:492  City:santa rosa  Url:http://api.openweathermap.org/data/2.5/weather?q=santa rosa&appid=api_key&units=imperial
    Count:493  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:494  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:495  City:champerico  Url:http://api.openweathermap.org/data/2.5/weather?q=champerico&appid=api_key&units=imperial
    Count:496  City:saleaula  Url:http://api.openweathermap.org/data/2.5/weather?q=saleaula&appid=api_key&units=imperial
    Count:497  City:oranjemund  Url:http://api.openweathermap.org/data/2.5/weather?q=oranjemund&appid=api_key&units=imperial
    Count:498  City:dunedin  Url:http://api.openweathermap.org/data/2.5/weather?q=dunedin&appid=api_key&units=imperial
    Count:499  City:vitim  Url:http://api.openweathermap.org/data/2.5/weather?q=vitim&appid=api_key&units=imperial
    Count:500  City:hobart  Url:http://api.openweathermap.org/data/2.5/weather?q=hobart&appid=api_key&units=imperial
    Count:501  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:502  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:503  City:bethel  Url:http://api.openweathermap.org/data/2.5/weather?q=bethel&appid=api_key&units=imperial
    Count:504  City:lebu  Url:http://api.openweathermap.org/data/2.5/weather?q=lebu&appid=api_key&units=imperial
    Count:505  City:gumusyaka  Url:http://api.openweathermap.org/data/2.5/weather?q=gumusyaka&appid=api_key&units=imperial
    Count:506  City:port alfred  Url:http://api.openweathermap.org/data/2.5/weather?q=port alfred&appid=api_key&units=imperial
    Count:507  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:508  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:509  City:faya  Url:http://api.openweathermap.org/data/2.5/weather?q=faya&appid=api_key&units=imperial
    Count:510  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:511  City:attawapiskat  Url:http://api.openweathermap.org/data/2.5/weather?q=attawapiskat&appid=api_key&units=imperial
    Count:512  City:bluff  Url:http://api.openweathermap.org/data/2.5/weather?q=bluff&appid=api_key&units=imperial
    Count:513  City:saint-joseph  Url:http://api.openweathermap.org/data/2.5/weather?q=saint-joseph&appid=api_key&units=imperial
    Count:514  City:paka  Url:http://api.openweathermap.org/data/2.5/weather?q=paka&appid=api_key&units=imperial
    Count:515  City:cherskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=cherskiy&appid=api_key&units=imperial
    Count:516  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:517  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:518  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:519  City:port elizabeth  Url:http://api.openweathermap.org/data/2.5/weather?q=port elizabeth&appid=api_key&units=imperial
    Count:520  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:521  City:ponta do sol  Url:http://api.openweathermap.org/data/2.5/weather?q=ponta do sol&appid=api_key&units=imperial
    Count:522  City:sogne  Url:http://api.openweathermap.org/data/2.5/weather?q=sogne&appid=api_key&units=imperial
    Count:523  City:torbay  Url:http://api.openweathermap.org/data/2.5/weather?q=torbay&appid=api_key&units=imperial
    Count:524  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:525  City:biak  Url:http://api.openweathermap.org/data/2.5/weather?q=biak&appid=api_key&units=imperial
    Count:526  City:sitka  Url:http://api.openweathermap.org/data/2.5/weather?q=sitka&appid=api_key&units=imperial
    Count:527  City:tuensang  Url:http://api.openweathermap.org/data/2.5/weather?q=tuensang&appid=api_key&units=imperial
    Count:528  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:529  City:ancud  Url:http://api.openweathermap.org/data/2.5/weather?q=ancud&appid=api_key&units=imperial
    Count:530  City:sao filipe  Url:http://api.openweathermap.org/data/2.5/weather?q=sao filipe&appid=api_key&units=imperial
    Count:531  City:andzhiyevskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=andzhiyevskiy&appid=api_key&units=imperial
    Count:532  City:longyearbyen  Url:http://api.openweathermap.org/data/2.5/weather?q=longyearbyen&appid=api_key&units=imperial
    Count:533  City:ribeira grande  Url:http://api.openweathermap.org/data/2.5/weather?q=ribeira grande&appid=api_key&units=imperial
    Count:534  City:vestmannaeyjar  Url:http://api.openweathermap.org/data/2.5/weather?q=vestmannaeyjar&appid=api_key&units=imperial
    Count:535  City:garoua  Url:http://api.openweathermap.org/data/2.5/weather?q=garoua&appid=api_key&units=imperial
    Count:536  City:dhidhdhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=dhidhdhoo&appid=api_key&units=imperial
    Count:537  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:538  City:castro  Url:http://api.openweathermap.org/data/2.5/weather?q=castro&appid=api_key&units=imperial
    Count:539  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:540  City:esperance  Url:http://api.openweathermap.org/data/2.5/weather?q=esperance&appid=api_key&units=imperial
    Count:541  City:ranong  Url:http://api.openweathermap.org/data/2.5/weather?q=ranong&appid=api_key&units=imperial
    Count:542  City:fortuna  Url:http://api.openweathermap.org/data/2.5/weather?q=fortuna&appid=api_key&units=imperial
    Count:543  City:cape town  Url:http://api.openweathermap.org/data/2.5/weather?q=cape town&appid=api_key&units=imperial
    Count:544  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:545  City:vaini  Url:http://api.openweathermap.org/data/2.5/weather?q=vaini&appid=api_key&units=imperial
    Count:546  City:usinsk  Url:http://api.openweathermap.org/data/2.5/weather?q=usinsk&appid=api_key&units=imperial
    Count:547  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:548  City:el sauzal  Url:http://api.openweathermap.org/data/2.5/weather?q=el sauzal&appid=api_key&units=imperial
    Count:549  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:550  City:tsihombe  Url:http://api.openweathermap.org/data/2.5/weather?q=tsihombe&appid=api_key&units=imperial
    Count:551  City:sri aman  Url:http://api.openweathermap.org/data/2.5/weather?q=sri aman&appid=api_key&units=imperial
    Count:552  City:walvis bay  Url:http://api.openweathermap.org/data/2.5/weather?q=walvis bay&appid=api_key&units=imperial
    Count:553  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:554  City:norsup  Url:http://api.openweathermap.org/data/2.5/weather?q=norsup&appid=api_key&units=imperial
    Count:555  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:556  City:puerto ayora  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto ayora&appid=api_key&units=imperial
    Count:557  City:ixtapa  Url:http://api.openweathermap.org/data/2.5/weather?q=ixtapa&appid=api_key&units=imperial
    Count:558  City:tuktoyaktuk  Url:http://api.openweathermap.org/data/2.5/weather?q=tuktoyaktuk&appid=api_key&units=imperial
    Count:559  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:560  City:puerto escondido  Url:http://api.openweathermap.org/data/2.5/weather?q=puerto escondido&appid=api_key&units=imperial
    Count:561  City:udachnyy  Url:http://api.openweathermap.org/data/2.5/weather?q=udachnyy&appid=api_key&units=imperial
    Count:562  City:ribeira grande  Url:http://api.openweathermap.org/data/2.5/weather?q=ribeira grande&appid=api_key&units=imperial
    Count:563  City:vallenar  Url:http://api.openweathermap.org/data/2.5/weather?q=vallenar&appid=api_key&units=imperial
    Count:564  City:oussouye  Url:http://api.openweathermap.org/data/2.5/weather?q=oussouye&appid=api_key&units=imperial
    Count:565  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:566  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:567  City:east london  Url:http://api.openweathermap.org/data/2.5/weather?q=east london&appid=api_key&units=imperial
    Count:568  City:boromlya  Url:http://api.openweathermap.org/data/2.5/weather?q=boromlya&appid=api_key&units=imperial
    Count:569  City:yellowknife  Url:http://api.openweathermap.org/data/2.5/weather?q=yellowknife&appid=api_key&units=imperial
    Count:570  City:hilo  Url:http://api.openweathermap.org/data/2.5/weather?q=hilo&appid=api_key&units=imperial
    Count:571  City:oravita  Url:http://api.openweathermap.org/data/2.5/weather?q=oravita&appid=api_key&units=imperial
    Count:572  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:573  City:hithadhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&appid=api_key&units=imperial
    Count:574  City:ahipara  Url:http://api.openweathermap.org/data/2.5/weather?q=ahipara&appid=api_key&units=imperial
    Count:575  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:576  City:busselton  Url:http://api.openweathermap.org/data/2.5/weather?q=busselton&appid=api_key&units=imperial
    Count:577  City:taolanaro  Url:http://api.openweathermap.org/data/2.5/weather?q=taolanaro&appid=api_key&units=imperial
    Count:578  City:ushuaia  Url:http://api.openweathermap.org/data/2.5/weather?q=ushuaia&appid=api_key&units=imperial
    Count:579  City:kodiak  Url:http://api.openweathermap.org/data/2.5/weather?q=kodiak&appid=api_key&units=imperial
    Count:580  City:kasane  Url:http://api.openweathermap.org/data/2.5/weather?q=kasane&appid=api_key&units=imperial
    Count:581  City:mar del plata  Url:http://api.openweathermap.org/data/2.5/weather?q=mar del plata&appid=api_key&units=imperial
    Count:582  City:hermanus  Url:http://api.openweathermap.org/data/2.5/weather?q=hermanus&appid=api_key&units=imperial
    Count:583  City:berlevag  Url:http://api.openweathermap.org/data/2.5/weather?q=berlevag&appid=api_key&units=imperial
    Count:584  City:atuona  Url:http://api.openweathermap.org/data/2.5/weather?q=atuona&appid=api_key&units=imperial
    Count:585  City:khatanga  Url:http://api.openweathermap.org/data/2.5/weather?q=khatanga&appid=api_key&units=imperial
    Count:586  City:tazovskiy  Url:http://api.openweathermap.org/data/2.5/weather?q=tazovskiy&appid=api_key&units=imperial
    Count:587  City:east london  Url:http://api.openweathermap.org/data/2.5/weather?q=east london&appid=api_key&units=imperial
    Count:588  City:westport  Url:http://api.openweathermap.org/data/2.5/weather?q=westport&appid=api_key&units=imperial
    Count:589  City:zolotinka  Url:http://api.openweathermap.org/data/2.5/weather?q=zolotinka&appid=api_key&units=imperial
    Count:590  City:shahr-e kord  Url:http://api.openweathermap.org/data/2.5/weather?q=shahr-e kord&appid=api_key&units=imperial
    Count:591  City:araouane  Url:http://api.openweathermap.org/data/2.5/weather?q=araouane&appid=api_key&units=imperial
    Count:592  City:meghri  Url:http://api.openweathermap.org/data/2.5/weather?q=meghri&appid=api_key&units=imperial
    Count:593  City:barrow  Url:http://api.openweathermap.org/data/2.5/weather?q=barrow&appid=api_key&units=imperial
    Count:594  City:roebourne  Url:http://api.openweathermap.org/data/2.5/weather?q=roebourne&appid=api_key&units=imperial
    Count:595  City:castro  Url:http://api.openweathermap.org/data/2.5/weather?q=castro&appid=api_key&units=imperial
    Count:596  City:shubarkuduk  Url:http://api.openweathermap.org/data/2.5/weather?q=shubarkuduk&appid=api_key&units=imperial
    Count:597  City:severo-kurilsk  Url:http://api.openweathermap.org/data/2.5/weather?q=severo-kurilsk&appid=api_key&units=imperial
    Count:598  City:kaitangata  Url:http://api.openweathermap.org/data/2.5/weather?q=kaitangata&appid=api_key&units=imperial
    Count:599  City:trairi  Url:http://api.openweathermap.org/data/2.5/weather?q=trairi&appid=api_key&units=imperial
    Count:600  City:punta arenas  Url:http://api.openweathermap.org/data/2.5/weather?q=punta arenas&appid=api_key&units=imperial
    Count:601  City:tasiilaq  Url:http://api.openweathermap.org/data/2.5/weather?q=tasiilaq&appid=api_key&units=imperial
    Count:602  City:ilhabela  Url:http://api.openweathermap.org/data/2.5/weather?q=ilhabela&appid=api_key&units=imperial
    Count:603  City:mataura  Url:http://api.openweathermap.org/data/2.5/weather?q=mataura&appid=api_key&units=imperial
    Count:604  City:dinguiraye  Url:http://api.openweathermap.org/data/2.5/weather?q=dinguiraye&appid=api_key&units=imperial
    Count:605  City:baker city  Url:http://api.openweathermap.org/data/2.5/weather?q=baker city&appid=api_key&units=imperial
    Count:606  City:meulaboh  Url:http://api.openweathermap.org/data/2.5/weather?q=meulaboh&appid=api_key&units=imperial
    Count:607  City:nikolskoye  Url:http://api.openweathermap.org/data/2.5/weather?q=nikolskoye&appid=api_key&units=imperial
    Count:608  City:thompson  Url:http://api.openweathermap.org/data/2.5/weather?q=thompson&appid=api_key&units=imperial
    Count:609  City:barentsburg  Url:http://api.openweathermap.org/data/2.5/weather?q=barentsburg&appid=api_key&units=imperial
    Count:610  City:ponta do sol  Url:http://api.openweathermap.org/data/2.5/weather?q=ponta do sol&appid=api_key&units=imperial
    Count:611  City:vermillion  Url:http://api.openweathermap.org/data/2.5/weather?q=vermillion&appid=api_key&units=imperial
    Count:612  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:613  City:new norfolk  Url:http://api.openweathermap.org/data/2.5/weather?q=new norfolk&appid=api_key&units=imperial
    Count:614  City:jamestown  Url:http://api.openweathermap.org/data/2.5/weather?q=jamestown&appid=api_key&units=imperial
    Count:615  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial
    Count:616  City:sept-iles  Url:http://api.openweathermap.org/data/2.5/weather?q=sept-iles&appid=api_key&units=imperial
    Count:617  City:hithadhoo  Url:http://api.openweathermap.org/data/2.5/weather?q=hithadhoo&appid=api_key&units=imperial
    Count:618  City:kemijarvi  Url:http://api.openweathermap.org/data/2.5/weather?q=kemijarvi&appid=api_key&units=imperial
    Count:619  City:lebu  Url:http://api.openweathermap.org/data/2.5/weather?q=lebu&appid=api_key&units=imperial
    Count:620  City:rikitea  Url:http://api.openweathermap.org/data/2.5/weather?q=rikitea&appid=api_key&units=imperial



```python
#Take urls and pass them through for loop to get json data for cities. 
#Pass over any response that does not bring back data.
weather_json_l=[]
for city in url_list:
    response = req.get(city).json()
    if response=={'cod': '404', 'message': 'city not found'}:
        continue
    weather_json_l.append(response)
    
print(len(weather_json_l))
```

    561



```python
if len(weather_json_l)<500:
    print("Add more locations")
else:
    print("Locations greater than 500")
```

    Locations greater than 500



```python
#Get Data for DataFrame
lat_data=[data.get("coord").get("lat") for data in weather_json_l]
temp_data=[data.get("main").get("temp") for data in weather_json_l]
city=[data.get("name") for data in weather_json_l]
humidity=[data.get("main").get("humidity") for data in weather_json_l]
wind_speed=[data.get('wind').get('speed') for data in weather_json_l]
cloud=[data.get('clouds').get('all')for data in weather_json_l]
country=[data.get('sys').get('country')for data in weather_json_l]
date=[data.get('dt') for data in weather_json_l]
```


```python
#Create DataFrame for weather plots
#Count number of cities to ensure over 500
#Write DataFrame to csv file
weather_dict = {"Temperature(F)": temp_data,
                "Latitude":lat_data,
                "City":city,
                "Humidity":humidity,
                "Wind Speed":wind_speed,
                "Cloudiness":cloud,
                "Country Code":country
               }
weather_df=pd.DataFrame(weather_dict)
weather_df.set_index("City", inplace=True)
weather_df.to_csv('weather_data.csv')
weather_df.head()

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
      <th>Cloudiness</th>
      <th>Country Code</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Temperature(F)</th>
      <th>Wind Speed</th>
    </tr>
    <tr>
      <th>City</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Luanda</th>
      <td>76</td>
      <td>AO</td>
      <td>100</td>
      <td>-8.83</td>
      <td>75.29</td>
      <td>4.18</td>
    </tr>
    <tr>
      <th>Cabedelo</th>
      <td>100</td>
      <td>BR</td>
      <td>99</td>
      <td>-6.97</td>
      <td>79.21</td>
      <td>10.33</td>
    </tr>
    <tr>
      <th>Bredasdorp</th>
      <td>0</td>
      <td>ZA</td>
      <td>100</td>
      <td>-34.53</td>
      <td>60.94</td>
      <td>7.54</td>
    </tr>
    <tr>
      <th>Chuy</th>
      <td>32</td>
      <td>UY</td>
      <td>88</td>
      <td>-33.69</td>
      <td>66.29</td>
      <td>7.87</td>
    </tr>
    <tr>
      <th>Tanete</th>
      <td>36</td>
      <td>ID</td>
      <td>68</td>
      <td>-3.94</td>
      <td>88.25</td>
      <td>6.76</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create parameters for scatter plots
temperature=weather_df['Temperature(F)']
latitude=weather_df['Latitude']
humidity=weather_df['Humidity']
cloudiness=weather_df['Cloudiness']
wind_speed=weather_df['Wind Speed']
```


```python
#Create Temp vs. Latitude plot
plt.scatter(y=temperature,x=latitude,c='blue',edgecolors='black', linewidth=1.0)
plt.title('City Latitude vs. Max Temperature (02/26/2018)', fontsize=14)
plt.xlabel('Latitude', fontsize=10)
plt.ylabel('Max Temperature (F)',fontsize=10)
plt.xlim(-80,100)
plt.ylim(-100,150)
sns.set()
plt.savefig('City Latitude vs Max Temperature.png')
plt.show()
```


![png](output_14_0.png)



```python
#Create Humidity vs. Latitude plot
plt.scatter(y=humidity,x=latitude,c='blue',edgecolor='black',linewidth=1.0)
plt.title('City Latitude vs. Humidity (02/26/2018)', fontsize=14)
plt.xlabel('Latitude', fontsize=10)
plt.ylabel('Humidity (%)',fontsize=10)
plt.xlim(-80,100)
plt.ylim(-20,120)
sns.set()
plt.savefig('City Latitude vs Humidity.png')
plt.show()
```


![png](output_15_0.png)



```python
#Create Cloudiness vs. Latitude plot
plt.scatter(y=cloudiness,x=latitude,c='blue',edgecolor='black',linewidth=1.0)
plt.title('City Latitude vs. Cloudiness (02/26/2018)', fontsize=14)
plt.xlabel('Latitude', fontsize=10)
plt.ylabel('Cloudiness (%)',fontsize=10)
plt.xlim(-80,100)
plt.ylim(-20,120)
sns.set()
plt.savefig('City Latitude vs Cloudiness.png')
plt.show()
```


![png](output_16_0.png)



```python
#Create Wind Speed vs. Latitude plot
plt.scatter(y=wind_speed,x=latitude,c='blue',edgecolor='black',linewidth=1.0)
plt.title('City Latitude vs. Wind Speed (02/26/2018)', fontsize=14)
plt.xlabel('Latitude', fontsize=10)
plt.ylabel('Wind Speed (mph)',fontsize=10)
plt.ylim(-5,40)
plt.xlim(-80,100)
sns.set()
plt.savefig('City Latitude vs Wind Speed.png')
plt.show()
```


![png](output_17_0.png)

