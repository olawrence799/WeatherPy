
# Observed Trends
    1) Maximum temperatures observed by city tend to max out around cities with latitude close to zero. Observed temperatures also appear more closely clustered around latitude = 0, and vary more as latitudes move further away from zero.
    
    2) There are clusters of observed city humidities at 100% and 0% within the latitude range 60-80. This sample also contains cities with humidity at 100% across nearly the entire latitude range.
    
    3) Observed wind speeds have the most data points with speed below 10 mph in cities with positive latitudes (>0), though wind speed ranges are similar across all latitudes.


```python
from citipy import citipy
import requests
import json
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
```


```python
file = '../weather_apikey'

def get_file_contents(file):
    """ Given a filename,
        return the contents of that file
    """
    try:
        with open(file, 'r') as f:
            # It's assumed our file contains a single line,
            # with our API key
            return f.read().strip()
    except FileNotFoundError:
        print("'%s' file not found" % file)
```


```python
api_key = get_file_contents(file)
api_key = api_key[-33:-1]
#print("Our API key is: %s" % (api_key))
```


```python
lats = np.random.uniform(-90, 91, size=55)
lngs = np.random.uniform(-180, 181, size=55)
#lats = range(-90, 91)
#lngs = range(-180, 181)

city_list = []

for lat in lats:
    for lng in lngs:
        city = citipy.nearest_city(lat, lng)
        city_list.append(city)

city_pd = pd.DataFrame(city_list)
sampled_cities = city_pd.sample(3000)
sampled_cities.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3000</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>841</td>
    </tr>
    <tr>
      <th>top</th>
      <td>&lt;citipy.citipy.City object at 0x1092f3eb8&gt;</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>100</td>
    </tr>
  </tbody>
</table>
</div>




```python
sampled_cities = sampled_cities[0].unique()
```


```python
target_urls = []
for city in sampled_cities:
    name = city.city_name
    country_code = city.country_code
    target_url = "http://api.openweathermap.org/data/2.5/weather" \
    +"?"+"units=IMPERIAL&mode=json&APPID="+api_key+"&q="+name+","+country_code
    target_urls.append(target_url)
    print(target_url)
```

    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pevek,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nikolskoye,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rovaniemi,fi
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hilo,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=porohy,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rikitea,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sorland,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=busselton,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san cristobal,ec
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bredasdorp,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khatanga,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=east london,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ribeira grande,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hermanus,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kaitangata,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bluff,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mataura,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hobart,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=poronaysk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-augustin,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sao filipe,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=esperance,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chuy,uy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kununurra,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=edd,er
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dikson,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nanakuli,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=loreto,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-francois,gp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bantry,ie
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=taolanaro,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=laguna,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=new norfolk,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vaini,to
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=boguchar,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=clyde river,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nara,ml
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sao paulo de olivenca,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=naze,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=illoqqortoormiut,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cape town,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=malindi,ke
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mardin,tr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ayan,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=palmer,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kazalinsk,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=arraial do cabo,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=castro,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dzilam gonzalez,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jamestown,sh
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bani,do
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tsihombe,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=anloga,gh
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=upernavik,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=beloha,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kenai,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=longyearbyen,sj
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tuktoyaktuk,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=airai,pw
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marienburg,sr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san patricio,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=necochea,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=butaritari,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ponta do sol,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kapaa,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vanimo,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=buta,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kutum,sd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ostrovnoy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port blair,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kuche,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=menongue,ao
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=luderitz,na
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saleaula,ws
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=severo-kurilsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kavieng,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jiaohe,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dingle,ie
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kodiak,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=warmbad,na
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marystown,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=raga,sd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ushuaia,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tulsipur,np
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=souillac,mu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shimoda,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=qaanaaq,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hobyo,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=juli,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zachagansk,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chokurdakh,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint george,bm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=faya,td
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=belushya guba,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cidreira,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=prince rupert,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khormuj,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mys shmidta,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port alfred,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nome,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rantepao,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=medicine hat,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bethel,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yellowknife,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=richards bay,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=leningradskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vaitupu,wf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=siderno,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kruisfontein,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=altay,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=avarua,ck
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=puerto ayora,ec
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=verkhnyaya inta,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tumannyy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mubi,ng
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=provideniya,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cervo,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=svetlogorsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=letterkenny,ie
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saskylakh,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=neuquen,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=havre,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=karamea,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=moissala,td
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=punta arenas,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rawannawi,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nizhneyansk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=miyako,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rio gallegos,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=angoche,mz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=carnarvon,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port-cartier,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barrow,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=puerto del rosario,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=leopoldov,sk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=namie,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=buariki,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lavrentiya,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=albany,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=makat,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bairiki,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tuatapere,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=fortuna,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=plastun,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=heihe,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lebu,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pobe,bj
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=iqaluit,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=melendugno,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=touros,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=biak,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=broken hill,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bathsheba,bb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mana,gf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=harper,lr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=orange walk,bz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ribeira brava,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=teluk nibung,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=thunder bay,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hambantota,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zbarazh,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grindavik,is
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=coihaique,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gelemso,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=eyl,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=trincomalee,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bonfim,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vacha,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kars,tr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=santa cruz,cr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gerash,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=acarau,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=los llanos de aridane,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=korem,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mount gambier,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=anadyr,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kahului,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=abha,sa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aksu,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=forestville,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vao,nc
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=praia,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tomatlan,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yulara,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nuseni,ro
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hithadhoo,mv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sabang,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=miri,my
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=oistins,bb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dunedin,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mareeba,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=potam,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bolungarvik,is
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=merville,fr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lagoa,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=atuona,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aitape,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=piacabucu,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=algiers,dz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hami,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sola,vu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mar del plata,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hihifo,to
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=baykit,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=durango,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=paducah,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=egvekinot,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=iquitos,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=samusu,ws
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=boyolangu,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=halalo,wf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=deputatskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mercedes,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grand river south east,mu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=matara,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tasiilaq,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=umm lajj,sa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tiznit,ma
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=waupun,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kuching,my
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vila velha,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=satun,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chivolo,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=szabadbattyan,hu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=beringovskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=thompson,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=batticaloa,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sao jose da coroa grande,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nanortalik,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=satitoa,ws
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vestmanna,fo
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kidal,ml
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=corner brook,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ginir,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bengkulu,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=fano,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gorontalo,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gambo,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=awbari,ly
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bilma,ne
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=forio,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bereda,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=waddan,ly
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kabo,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sciacca,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aztec,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tomigusuku,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ambon,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=antalaha,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=solnechnyy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=petropavlovsk-kamchatskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=macusani,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lewistown,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grand centre,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pacific grove,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nechi,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=umzimvubu,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=torbay,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=porto novo,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rawson,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=caceres,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yakeshi,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=novikovo,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=glubokiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jalu,ly
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vakhrushev,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=havoysund,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=joniskelis,lt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tairua,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=buraydah,sa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rungata,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aljezur,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ahuimanu,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ust-tsilma,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maragogi,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sao bartolomeu de messines,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=beisfjord,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nevelsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tiksi,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ixtapa,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=qingdao,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=seoul,kr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jilib,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sisimiut,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=namibe,ao
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=waipawa,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=burnie,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=princeton,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hasaki,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cabo san lucas,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=charyshskoye,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=broome,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khandyga,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jiwani,pk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lola,gn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=balkanabat,tm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=camopi,gf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chingirlau,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aklavik,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=krasnoselkup,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nantucket,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sabha,ly
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bandar-e lengeh,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=batagay,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gurgan,az
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lincoln,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=codrington,ag
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mabaruma,gy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=plettenberg bay,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=meulaboh,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=adrar,dz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pathein,mm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kyabe,td
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mukhen,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=diffa,ne
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barcelona,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ahipara,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cortez,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bubaque,gw
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=craig,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kirakira,sb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mitake,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ondorhaan,mn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=corbu,ro
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=save,bj
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kinsale,ie
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=guiratinga,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sentyabrskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=buala,sb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aykhal,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dongsheng,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mayo,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=abong mbang,cm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nabire,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=amderma,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wokha,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ornskoldsvik,se
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port shepstone,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=waingapu,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=svetlyy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chapais,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rafai,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san policarpo,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=albert,fr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=santo angelo,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=band,hu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mitu,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lamu,ke
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lethem,gy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=novyy yarychiv,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sakakah,sa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=makokou,ga
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marco,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maldonado,uy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sangar,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=husavik,is
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ulladulla,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=miles city,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=xingcheng,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kichera,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cockburn town,tc
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kurchum,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=fomboni,km
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=surovikino,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=filotion,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yuncheng,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phetchaburi,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=xining,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=honningsvag,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kalmunai,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=senanga,zm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jijiga,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=isangel,vu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ketchikan,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=falealupo,ws
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=siocon,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lazaro cardenas,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=les escoumins,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yutsa,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=victoria,sc
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vanino,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tabiauea,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pemba,mz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=etchoropo,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=salalah,om
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bargal,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khonuu,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barentsburg,sj
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=panzhihua,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=katunino,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-pierre,pm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kattivakkam,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=price,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=poum,nc
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=guadalupe y calvo,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nazarovo,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=solsona,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shahriyar,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tromso,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=colombia,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phek,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tansen,np
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bambous virieux,mu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ishinomaki,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=katsuura,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=asau,tv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mbaiki,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=huaihua,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=erenhot,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=strzelce opolskie,pl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint anthony,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=praia da vitoria,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=atar,mr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=acari,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=elizabeth city,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=voljevac,ba
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=fairbanks,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=santarem,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=abu kamal,sy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lixourion,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yumen,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zernograd,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=payakumbuh,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=olafsvik,is
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pfarrkirchen,de
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=segovia,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jequie,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rayagada,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ouallam,ne
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=batangafo,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=union,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=talnakh,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=prainha,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sarakhs,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=stefan cel mare,ro
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dok kham tai,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=karaton,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=claveria,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gogosari,ro
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khani,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=labelle,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=glubokoe,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=murgab,tm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=inderborskiy,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-joseph,re
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=viedma,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=naron,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mullaitivu,lk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=carutapera,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=amalapuram,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=puerto rico,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zlate moravce,sk
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=westport,ie
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=moron,mn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wanning,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cherskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dicabisagan,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san rafael del sur,ni
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mugur-aksy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kisangani,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=senov,cz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sitrah,bh
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=batagay-alyta,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bandarbeyla,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ginda,er
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nioki,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=solano,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=palaia fokaia,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=portland,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ust-kamchatsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=swift current,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=simbahan,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vila franca do campo,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=louisbourg,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sambava,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hofn,is
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saldanha,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jackson,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=te anau,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=georgetown,gy
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vitkov,cz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=puerto baquerizo moreno,ec
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shimminato,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=benguela,ao
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tonekabon,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=narsaq,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chitrakonda,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lakatoro,vu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bandundu,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=makakilo city,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=georgetown,sh
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wulanhaote,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=arrecife,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=suileng,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tornio,fi
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kindu,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phan thiet,vn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=geraldton,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=namatanai,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vorontsovka,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=santa marta,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port keats,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=aktau,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chapada dos guimaraes,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barahan,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=linjiang,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=terra santa,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=virginia beach,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=liku,wf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shingu,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=namwala,zm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san vicente,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=arrifes,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=raudeberg,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dohrighat,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=victor harbor,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=les cayes,ht
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khor,qa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=suzu,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kabompo,zm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=srednekolymsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=snasa,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nishihara,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=carballo,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=evensk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cavite,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=thayetmyo,mm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=coquimbo,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=erzin,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=anito,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maniitsoq,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san jeronimo,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nago,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san lawrenz,mt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sarajevo,ba
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=northam,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hoopstad,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phan rang,vn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=karauzyak,uz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=leh,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=qui nhon,vn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=punta gorda,bz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=stribro,cz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ust-nera,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ati,td
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=svetlaya,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=quarteira,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lubyany,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lodja,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mazamet,fr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dubai,ae
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=damghan,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sur,om
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=naryan-mar,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pombas,cv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dostpur,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=qixia,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kinablangan,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mahina,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=iwaki,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=moyale,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grand-santi,gf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=lorengau,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kasempa,zm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tura,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=landskrona,se
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=palabuhanratu,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ryotsu,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bandar-e torkaman,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=iskateley,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=okha,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=labuhan,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jinxiang,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=xinzhi,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=urdzhar,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zabid,ye
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=preobrazhenskaya,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=byron bay,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=deer lake,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=machhlishahr,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cascais,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=quibor,ve
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=karaul,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-philippe,re
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=guanare,ve
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=coahuayana,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nova olimpia,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yangambi,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=malayal,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=otaru,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zeya,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=soller,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san isidro,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kamenskoye,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sitka,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=crotone,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=fare,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maghama,mr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=buchanan,lr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tura,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mnogovershinnyy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=alyangula,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yining,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maues,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kati,ml
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kamarion,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tautira,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khilok,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=de-kastri,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tignere,cm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=college,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=teknaf,bd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jizan,sa
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wajir,ke
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=myanaung,mm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mbandaka,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gander,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mamboma,sl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mongoumba,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ilulissat,gl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=poli,cm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jeremie,ht
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tiarei,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=verenchanka,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dali,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tashara,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=balykshi,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zhangye,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=impfondo,cg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=comodoro rivadavia,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tymovskoye,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zhigansk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=silopi,tr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kuytun,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rokiskis,lt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sungaipenuh,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sobolevo,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gloversville,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bang rakam,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=burgeo,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barkly west,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=basoko,cd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zaysan,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mocambique,mz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=amursk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=el paraiso,hn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=samarai,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=goundi,td
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phatthalung,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kitami,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=petrvald,cz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cayenne,gf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ylivieska,fi
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yarada,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=laiyang,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sibu,my
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=muros,es
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=muhos,fi
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gandorhun,sl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bembereke,bj
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ternate,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ponta delgada,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gazojak,tm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=phrae,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dolores,ar
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mahebourg,mu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=barinas,ve
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zhovtneve,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rock sound,bs
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=seymchan,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=akyab,mm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tidore,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maceio,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mouzakion,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wladyslawowo,pl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chicama,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tuy hoa,vn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sanchez,do
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port elizabeth,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=birin,dz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kyra,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kamariotissa,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=faanui,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jiexiu,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port hardy,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=eureka,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=suphan buri,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wyndham,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=huarmey,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=khash,ir
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=san pedro de macoris,do
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=noumea,nc
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tondela,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chunhuhub,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=saint-pascal,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=roald,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kedougou,sn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kolyvan,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=turukhansk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=borama,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=geraldton,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mayfield,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=karratha,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ewa beach,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=padang,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ijaki,ki
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kamenka,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=badiraguato,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=analipsis,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=urumqi,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cumaribo,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nhulunbuy,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=milos,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wanaka,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=estelle,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=beyla,gn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=genhe,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yele,sl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=oranjestad,aw
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bridlington,gb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=majagual,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=palu,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pangkalanbuun,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mlyniv,ua
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=puerto el triunfo,sv
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=basar,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kaeo,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bossangoa,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jakar,bt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grand gaube,mu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=banda aceh,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=uyar,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=acapulco,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port augusta,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ghazipur,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=port-gentil,ga
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ko samui,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kayes,ml
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=manuk mangkaw,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bayburt,tr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=serebryansk,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bloomington,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=hovd,mn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ruatoria,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=grants,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marsh harbour,bs
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=razole,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=klaksvik,fo
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vilyuysk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ivanishchi,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=serafimovich,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shchelyayur,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=say,ne
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wajid,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=winslow,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=psebay,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sosnogorsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=annaberg-buchholz,de
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ballarat,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shaoyang,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jackson,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=neftekamsk,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=guilin,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mossendjo,cg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nakhon si thammarat,th
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dahuk,iq
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kunashak,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vallenar,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cockburn town,bs
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=payo,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=meadow lake,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bilibino,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=worland,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=penzance,gb
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wenzhou,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ouranopolis,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=odienne,ci
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=yanam,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dinsor,so
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=quang ngai,vn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=birao,cf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=alofi,nu
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=umm kaddadah,sd
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=danane,ci
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maketu,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=pinega,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cravo norte,co
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ayacucho,pe
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bad hofgastein,at
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=suba,ph
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=stokmarknes,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marsa matruh,eg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=russell,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=gewane,et
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=oskemen,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=marzuq,ly
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=syracuse,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=wewak,pg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=le mee-sur-seine,fr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=beian,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=shaunavon,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=peniche,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bath,us
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=maryborough,au
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=faro,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bay roberts,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tongliao,cn
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tall kayf,iq
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=christchurch,nz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=cururupu,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=avera,pf
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=coihueco,cl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kolokani,ml
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=vizianagaram,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=jogighopa,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=dubovskoye,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rantauprapat,id
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=rabo de peixe,pt
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bay-khaak,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=opole,pl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zawadzkie,pl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mananara,mg
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bourges,fr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=bindi,sl
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=chunskiy,ru
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=nesna,no
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=itoman,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kazuno,jp
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=tanshui,tw
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=betsiamites,ca
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=creel,mx
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=sao joao da barra,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=kotaparh,in
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=labutta,mm
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=natal,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=margate,za
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=zhanaozen,kz
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=mouzouras,gr
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=almenara,br
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=erchie,it
    http://api.openweathermap.org/data/2.5/weather?units=IMPERIAL&mode=json&APPID=6f3d7c33078d605163112a422c1bd6a0&q=ambuclao,ph



```python
name = []
temp = []
humidity = []
clouds = []
wind = []
lat = []

for url in target_urls:
    city_weather = requests.get(url).json()
    if city_weather != {'cod': '404', 'message': 'city not found'}:
        city_name = city_weather['name']
        name.append(city_name)
        city_temp = city_weather['main']['temp_max']
        temp.append(city_temp)
        city_humidity = city_weather['main']['humidity']
        humidity.append(city_humidity)
        city_clouds = city_weather['clouds']['all']
        clouds.append(city_clouds)
        city_wind = city_weather['wind']['speed']
        wind.append(city_wind)
        city_lat = city_weather['coord']['lat']
        lat.append(city_lat)
        #print(json.dumps(city_weather, indent=2, sort_keys=True))
        #print(city_name, city_temp, city_humidity, city_clouds, city_wind, city_lat)
```


```python
city_data = {'City': name, 'Max Temperature (F)': temp, 'Humidity (%)': humidity,
        'Cloudiness (%)': clouds, 'Wind Speed (mph)': wind, 'Latitude': lat}
city_data = pd.DataFrame(city_data)
city_data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness (%)</th>
      <th>Humidity (%)</th>
      <th>Latitude</th>
      <th>Max Temperature (F)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Pevek</td>
      <td>12</td>
      <td>94</td>
      <td>69.70</td>
      <td>-11.76</td>
      <td>15.59</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nikolskoye</td>
      <td>75</td>
      <td>92</td>
      <td>59.70</td>
      <td>30.20</td>
      <td>22.37</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rovaniemi</td>
      <td>90</td>
      <td>84</td>
      <td>66.50</td>
      <td>8.60</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hilo</td>
      <td>20</td>
      <td>64</td>
      <td>19.71</td>
      <td>73.40</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Porohy</td>
      <td>88</td>
      <td>91</td>
      <td>48.24</td>
      <td>30.68</td>
      <td>12.57</td>
    </tr>
  </tbody>
</table>
</div>




```python
len(city_data)
```




    733




```python
city_data.to_csv("WeatherData.csv", index = False, header = True)
```


```python
plt.scatter(lat, temp, marker="o", linewidth = 1, edgecolor='black')
plt.title("City Latitude vs. Max Temperature (F) (12/22/17)")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")
plt.grid()
plt.show()
plt.savefig("LatitudeVsTemperature.png")
```


![png](output_11_0.png)



```python
plt.scatter(lat, humidity, marker="o", linewidth = 1, edgecolor='black')
plt.title("City Latitude vs. Humidity (%) (12/22/17)")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.grid()
plt.show()
plt.savefig("LatitudeVsHumidity.png")
```


![png](output_12_0.png)



```python
plt.scatter(lat, clouds, marker="o", linewidth = 1, edgecolor='black')
plt.title("City Latitude vs. Cloudiness (%) (12/22/17)")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.grid()
plt.show()
plt.savefig("LatitudeVsCloudiness.png")
```


![png](output_13_0.png)



```python
plt.scatter(lat, wind, marker="o", linewidth = 1, edgecolor='black')
plt.title("City Latitude vs. Wind Speed (mph) (12/22/17)")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
plt.grid()
plt.show()
plt.savefig("LatitudeVsWindSpeed.png")
```


![png](output_14_0.png)

# homework6
