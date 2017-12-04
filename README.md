
# WeatherPy
## Melvin Garcia


```python
# Import Dependencies
import matplotlib.pyplot as plt
from citipy import citipy
import requests as req
import pandas as pd
import numpy as np
import seaborn
import random
import apikeys
import json
```

## Generate Cities List


```python
# Create random set 2000 latitude and longitude values
# The large random size will allow more choices to choose from and prevent .nearest_city 
# from choosing the same city twice

randlon = np.random.uniform(low=-180, high=181, size=2000).tolist()
randlat = np.random.uniform(low=-90, high=91, size=2000).tolist()
```


```python
# Create list of city names to perform requests on

cities = []

for lat, lon in zip(randlat, randlon):
    city = citipy.nearest_city(lat, lon)
    if city not in cities:
        cities.append(city.city_name)
    else:
        city = citipy.nearest_city(lat, lon)
        cities.append(city.city_name)
```


```python
# Check that you have at least 500 unique city names to perform requests on
len(set(cities))
```




    754




```python
cities_unique = list(set(cities))
```

## Perform API Calls


```python
# Loop through the cities list and perform a request for data on each

url = "http://api.openweathermap.org/data/2.5/weather"

params = {'appid': apikeys.OWM_key,
          'q': '',
          'units': 'imperial'}

weather_data = {'City': [],
               'Cloudiness': [],
               'Country': [],
               'Date': [],
               'Humidity': [],
               'Lat': [],
               'Lng':[],
               'Max_Temp':[],
               'Wind Speed':[]}

city_count = 1
city_count_final = 501

print('Beginning Data Retrieval')
print('-----------------------------')

for city in cities_unique:
    try:
    # Get weather data
        params['q'] = city
        response = req.get(url, params=params).json()
    # Get weather params for df for at least 500 cities
        if city_count == city_count_final:
            print('-----------------------------')
            print('Data Retrieval Complete')
            print('-----------------------------')
            break
        elif city not in weather_data['City']:  
            # Construct weather_data dictionary
            weather_data['City'].append(response['name'])
            weather_data['Cloudiness'].append(response['clouds']['all'])
            weather_data['Country'].append(response['sys']['country'])
            weather_data['Date'].append(response['dt'])
            weather_data['Humidity'].append(response['main']['humidity'])
            weather_data['Lat'].append(response['coord']['lat'])
            weather_data['Lng'].append(response['coord']['lon'])
            weather_data['Max_Temp'].append(response['main']['temp_max'])
            weather_data['Wind Speed'].append(response['wind']['speed'])
            print(f'Processing Record {city_count} of 500 | {city}')
            print(f'http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID={apikeys.OWM_key}&q={city.replace(" ", "%20")}')
            city_count += 1
        elif city in weather_data['City']:
            continue
    except KeyError:
        print(f'{city} not found. Skipping...')
```

    Beginning Data Retrieval
    -----------------------------
    Processing Record 1 of 500 | nhulunbuy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nhulunbuy
    Processing Record 2 of 500 | kattivakkam
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kattivakkam
    Processing Record 3 of 500 | ciudad bolivar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ciudad%20bolivar
    Processing Record 4 of 500 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kavaratti
    Processing Record 5 of 500 | ampanihy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ampanihy
    Processing Record 6 of 500 | zhovti vody
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zhovti%20vody
    Processing Record 7 of 500 | labytnangi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=labytnangi
    Processing Record 8 of 500 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint-philippe
    Processing Record 9 of 500 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=chokurdakh
    Processing Record 10 of 500 | balkanabat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=balkanabat
    Processing Record 11 of 500 | korla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=korla
    Processing Record 12 of 500 | kambove
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kambove
    Processing Record 13 of 500 | sault sainte marie
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sault%20sainte%20marie
    Processing Record 14 of 500 | palu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=palu
    Processing Record 15 of 500 | yueyang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yueyang
    Processing Record 16 of 500 | hov
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hov
    Processing Record 17 of 500 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=barrow
    Processing Record 18 of 500 | broome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=broome
    tubruq not found. Skipping...
    sorvag not found. Skipping...
    Processing Record 19 of 500 | camocim
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=camocim
    Processing Record 20 of 500 | mareeba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mareeba
    Processing Record 21 of 500 | warrnambool
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=warrnambool
    Processing Record 22 of 500 | luganville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=luganville
    Processing Record 23 of 500 | tateyama
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tateyama
    Processing Record 24 of 500 | guiratinga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=guiratinga
    Processing Record 25 of 500 | predivinsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=predivinsk
    Processing Record 26 of 500 | segovia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=segovia
    Processing Record 27 of 500 | bontang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bontang
    Processing Record 28 of 500 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bredasdorp
    Processing Record 29 of 500 | san felipe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=san%20felipe
    rungata not found. Skipping...
    Processing Record 30 of 500 | barreirinhas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=barreirinhas
    Processing Record 31 of 500 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=richards%20bay
    Processing Record 32 of 500 | ahipara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ahipara
    Processing Record 33 of 500 | pisco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pisco
    Processing Record 34 of 500 | namibe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=namibe
    Processing Record 35 of 500 | linxia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=linxia
    Processing Record 36 of 500 | arlit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=arlit
    Processing Record 37 of 500 | chirongui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=chirongui
    jalu not found. Skipping...
    katsiveli not found. Skipping...
    Processing Record 38 of 500 | hays
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hays
    Processing Record 39 of 500 | carauari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=carauari
    nizhneyansk not found. Skipping...
    Processing Record 40 of 500 | porto nacional
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=porto%20nacional
    Processing Record 41 of 500 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saskylakh
    katunki not found. Skipping...
    kaitangata not found. Skipping...
    Processing Record 42 of 500 | saku
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saku
    Processing Record 43 of 500 | ambalavao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ambalavao
    Processing Record 44 of 500 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vila%20franca%20do%20campo
    Processing Record 45 of 500 | coihaique
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=coihaique
    Processing Record 46 of 500 | bagdarin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bagdarin
    Processing Record 47 of 500 | alenquer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=alenquer
    Processing Record 48 of 500 | kroya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kroya
    babanusah not found. Skipping...
    Processing Record 49 of 500 | rorvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rorvik
    macaboboni not found. Skipping...
    Processing Record 50 of 500 | kavieng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kavieng
    attawapiskat not found. Skipping...
    Processing Record 51 of 500 | murray bridge
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=murray%20bridge
    Processing Record 52 of 500 | santa lucia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=santa%20lucia
    Processing Record 53 of 500 | russkiy kameshkir
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=russkiy%20kameshkir
    Processing Record 54 of 500 | bethel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bethel
    Processing Record 55 of 500 | mehamn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mehamn
    Processing Record 56 of 500 | gashua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=gashua
    Processing Record 57 of 500 | moron
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=moron
    Processing Record 58 of 500 | yulara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yulara
    Processing Record 59 of 500 | filadelfia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=filadelfia
    Processing Record 60 of 500 | alice springs
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=alice%20springs
    Processing Record 61 of 500 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=port-gentil
    Processing Record 62 of 500 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=carnarvon
    Processing Record 63 of 500 | tambopata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tambopata
    Processing Record 64 of 500 | dikson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dikson
    Processing Record 65 of 500 | rutland
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rutland
    Processing Record 66 of 500 | breznik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=breznik
    Processing Record 67 of 500 | uyemskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=uyemskiy
    Processing Record 68 of 500 | upernavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=upernavik
    Processing Record 69 of 500 | san carlos de bariloche
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=san%20carlos%20de%20bariloche
    Processing Record 70 of 500 | boa vista
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=boa%20vista
    Processing Record 71 of 500 | porto novo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=porto%20novo
    Processing Record 72 of 500 | rio grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rio%20grande
    Processing Record 73 of 500 | hamilton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hamilton
    Processing Record 74 of 500 | legnica
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=legnica
    Processing Record 75 of 500 | aasiaat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aasiaat
    Processing Record 76 of 500 | mogadishu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mogadishu
    Processing Record 77 of 500 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=butaritari
    Processing Record 78 of 500 | sonari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sonari
    pousat not found. Skipping...
    Processing Record 79 of 500 | plaridel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=plaridel
    Processing Record 80 of 500 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tuatapere
    chenghai not found. Skipping...
    Processing Record 81 of 500 | cayenne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cayenne
    Processing Record 82 of 500 | severnyy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=severnyy
    Processing Record 83 of 500 | ankang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ankang
    Processing Record 84 of 500 | duluth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=duluth
    Processing Record 85 of 500 | fershampenuaz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fershampenuaz
    Processing Record 86 of 500 | pasighat
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pasighat
    Processing Record 87 of 500 | tsnori
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tsnori
    Processing Record 88 of 500 | berlevag
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=berlevag
    cumaribo not found. Skipping...
    Processing Record 89 of 500 | karasjok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=karasjok
    Processing Record 90 of 500 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=atuona
    Processing Record 91 of 500 | itoman
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=itoman
    Processing Record 92 of 500 | fukue
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fukue
    Processing Record 93 of 500 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mount%20gambier
    Processing Record 94 of 500 | eyl
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=eyl
    Processing Record 95 of 500 | hluti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hluti
    Processing Record 96 of 500 | madimba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=madimba
    Processing Record 97 of 500 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bengkulu
    viligili not found. Skipping...
    Processing Record 98 of 500 | carman
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=carman
    Processing Record 99 of 500 | romny
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=romny
    Processing Record 100 of 500 | tarko-sale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tarko-sale
    Processing Record 101 of 500 | college
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=college
    Processing Record 102 of 500 | antofagasta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=antofagasta
    chabahar not found. Skipping...
    Processing Record 103 of 500 | tezu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tezu
    Processing Record 104 of 500 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sao%20filipe
    lohkva not found. Skipping...
    Processing Record 105 of 500 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lebu
    dianopolis not found. Skipping...
    phrai bung not found. Skipping...
    Processing Record 106 of 500 | lompoc
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lompoc
    mys shmidta not found. Skipping...
    Processing Record 107 of 500 | nuuk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nuuk
    Processing Record 108 of 500 | calabozo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=calabozo
    Processing Record 109 of 500 | finschhafen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=finschhafen
    marcona not found. Skipping...
    Processing Record 110 of 500 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=beringovskiy
    bolshiye chapurniki not found. Skipping...
    Processing Record 111 of 500 | nantucket
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nantucket
    Processing Record 112 of 500 | oltu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=oltu
    Processing Record 113 of 500 | fortuna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fortuna
    Processing Record 114 of 500 | grand island
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=grand%20island
    Processing Record 115 of 500 | aden
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aden
    Processing Record 116 of 500 | lalian
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lalian
    Processing Record 117 of 500 | villarrica
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=villarrica
    Processing Record 118 of 500 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tuktoyaktuk
    Processing Record 119 of 500 | beloha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=beloha
    Processing Record 120 of 500 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sisimiut
    Processing Record 121 of 500 | goias
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=goias
    Processing Record 122 of 500 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ponta%20delgada
    Processing Record 123 of 500 | rocha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rocha
    Processing Record 124 of 500 | port hardy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=port%20hardy
    Processing Record 125 of 500 | zavoronezhskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zavoronezhskoye
    Processing Record 126 of 500 | misratah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=misratah
    Processing Record 127 of 500 | kalaleh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kalaleh
    Processing Record 128 of 500 | eagle pass
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=eagle%20pass
    tiruvottiyur not found. Skipping...
    Processing Record 129 of 500 | staryy nadym
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=staryy%20nadym
    Processing Record 130 of 500 | roma
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=roma
    Processing Record 131 of 500 | rabo de peixe
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rabo%20de%20peixe
    Processing Record 132 of 500 | merrill
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=merrill
    palabuhanratu not found. Skipping...
    Processing Record 133 of 500 | koumac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=koumac
    Processing Record 134 of 500 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yellowknife
    Processing Record 135 of 500 | tazmalt
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tazmalt
    Processing Record 136 of 500 | novoagansk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=novoagansk
    Processing Record 137 of 500 | vaitape
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vaitape
    Processing Record 138 of 500 | anadyr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=anadyr
    Processing Record 139 of 500 | port shepstone
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=port%20shepstone
    Processing Record 140 of 500 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kodiak
    Processing Record 141 of 500 | caravelas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=caravelas
    Processing Record 142 of 500 | kanniyakumari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kanniyakumari
    Processing Record 143 of 500 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bandarbeyla
    Processing Record 144 of 500 | hailar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hailar
    minudasht not found. Skipping...
    Processing Record 145 of 500 | zilair
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zilair
    Processing Record 146 of 500 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=clyde%20river
    Processing Record 147 of 500 | bubaque
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bubaque
    Processing Record 148 of 500 | bogdanovich
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bogdanovich
    Processing Record 149 of 500 | khorramshahr
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=khorramshahr
    Processing Record 150 of 500 | tarakan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tarakan
    Processing Record 151 of 500 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lavrentiya
    Processing Record 152 of 500 | oktyabrskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=oktyabrskiy
    Processing Record 153 of 500 | wuwei
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=wuwei
    wahran not found. Skipping...
    Processing Record 154 of 500 | aksu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aksu
    zhezkazgan not found. Skipping...
    Processing Record 155 of 500 | zyryanka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zyryanka
    Processing Record 156 of 500 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sinnamary
    Processing Record 157 of 500 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cap%20malheureux
    Processing Record 158 of 500 | shahr-e babak
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=shahr-e%20babak
    Processing Record 159 of 500 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=khatanga
    Processing Record 160 of 500 | vanavara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vanavara
    Processing Record 161 of 500 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=georgetown
    Processing Record 162 of 500 | nkan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nkan
    Processing Record 163 of 500 | pangody
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pangody
    Processing Record 164 of 500 | whitecourt
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=whitecourt
    Processing Record 165 of 500 | fuyu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fuyu
    Processing Record 166 of 500 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yar-sale
    gurskoye not found. Skipping...
    Processing Record 167 of 500 | ornskoldsvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ornskoldsvik
    Processing Record 168 of 500 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mar%20del%20plata
    Processing Record 169 of 500 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vila%20velha
    mrirt not found. Skipping...
    kuche not found. Skipping...
    tsihombe not found. Skipping...
    Processing Record 170 of 500 | shelburne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=shelburne
    Processing Record 171 of 500 | kununurra
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kununurra
    Processing Record 172 of 500 | salinas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=salinas
    Processing Record 173 of 500 | chuy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=chuy
    Processing Record 174 of 500 | shubarkuduk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=shubarkuduk
    abu samrah not found. Skipping...
    Processing Record 175 of 500 | the valley
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=the%20valley
    tatawin not found. Skipping...
    Processing Record 176 of 500 | albany
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=albany
    aflu not found. Skipping...
    Processing Record 177 of 500 | zhovtneve
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zhovtneve
    Processing Record 178 of 500 | mortka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mortka
    Processing Record 179 of 500 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=provideniya
    Processing Record 180 of 500 | lorengau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lorengau
    Processing Record 181 of 500 | la primavera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=la%20primavera
    nurobod not found. Skipping...
    Processing Record 182 of 500 | porto walter
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=porto%20walter
    kamenskoye not found. Skipping...
    Processing Record 183 of 500 | tete
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tete
    Processing Record 184 of 500 | kuching
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kuching
    Processing Record 185 of 500 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=egvekinot
    Processing Record 186 of 500 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=prince%20rupert
    Processing Record 187 of 500 | henderson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=henderson
    Processing Record 188 of 500 | aklavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aklavik
    Processing Record 189 of 500 | tilichiki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tilichiki
    Processing Record 190 of 500 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=torbay
    Processing Record 191 of 500 | tigzirt
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tigzirt
    Processing Record 192 of 500 | kondinskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kondinskoye
    Processing Record 193 of 500 | chepareria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=chepareria
    Processing Record 194 of 500 | banjar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=banjar
    Processing Record 195 of 500 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ostrovnoy
    Processing Record 196 of 500 | adrar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=adrar
    Processing Record 197 of 500 | nampula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nampula
    Processing Record 198 of 500 | talca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=talca
    Processing Record 199 of 500 | doka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=doka
    Processing Record 200 of 500 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=plettenberg%20bay
    Processing Record 201 of 500 | najran
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=najran
    Processing Record 202 of 500 | labuhan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=labuhan
    Processing Record 203 of 500 | trairi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=trairi
    Processing Record 204 of 500 | aviles
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aviles
    Processing Record 205 of 500 | vilanova del cami
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vilanova%20del%20cami
    tianmen not found. Skipping...
    Processing Record 206 of 500 | colares
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=colares
    Processing Record 207 of 500 | wanning
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=wanning
    Processing Record 208 of 500 | karratha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=karratha
    Processing Record 209 of 500 | maracaibo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=maracaibo
    Processing Record 210 of 500 | varhaug
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=varhaug
    Processing Record 211 of 500 | ulaanbaatar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ulaanbaatar
    Processing Record 212 of 500 | vardo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vardo
    Processing Record 213 of 500 | lazaro cardenas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lazaro%20cardenas
    Processing Record 214 of 500 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saldanha
    araguacu not found. Skipping...
    burkhala not found. Skipping...
    Processing Record 215 of 500 | laranjeiras do sul
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=laranjeiras%20do%20sul
    Processing Record 216 of 500 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=victoria
    Processing Record 217 of 500 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=santa%20cruz
    Processing Record 218 of 500 | road town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=road%20town
    Processing Record 219 of 500 | dauphin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dauphin
    Processing Record 220 of 500 | arrecife
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=arrecife
    Processing Record 221 of 500 | bhabua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bhabua
    duz not found. Skipping...
    Processing Record 222 of 500 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=isangel
    Processing Record 223 of 500 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=thompson
    Processing Record 224 of 500 | tigil
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tigil
    Processing Record 225 of 500 | conde
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=conde
    akyab not found. Skipping...
    valle de allende not found. Skipping...
    Processing Record 226 of 500 | leek
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=leek
    longlac not found. Skipping...
    Processing Record 227 of 500 | la tuque
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=la%20tuque
    Processing Record 228 of 500 | recsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=recsk
    Processing Record 229 of 500 | cassilandia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cassilandia
    Processing Record 230 of 500 | koygorodok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=koygorodok
    Processing Record 231 of 500 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=makakilo%20city
    Processing Record 232 of 500 | dwarka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dwarka
    Processing Record 233 of 500 | abha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=abha
    Processing Record 234 of 500 | mehtar lam
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mehtar%20lam
    Processing Record 235 of 500 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=banda%20aceh
    otukpo not found. Skipping...
    Processing Record 236 of 500 | nizhniy tsasuchey
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nizhniy%20tsasuchey
    Processing Record 237 of 500 | belyy yar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=belyy%20yar
    Processing Record 238 of 500 | ware
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ware
    Processing Record 239 of 500 | morwa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=morwa
    Processing Record 240 of 500 | todos santos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=todos%20santos
    Processing Record 241 of 500 | lar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lar
    Processing Record 242 of 500 | portales
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=portales
    belushya guba not found. Skipping...
    Processing Record 243 of 500 | ryomgard
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ryomgard
    Processing Record 244 of 500 | yala
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yala
    pafos not found. Skipping...
    Processing Record 245 of 500 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=severo-kurilsk
    Processing Record 246 of 500 | acari
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=acari
    Processing Record 247 of 500 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=jamestown
    Processing Record 248 of 500 | tanout
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tanout
    Processing Record 249 of 500 | lahad datu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lahad%20datu
    Processing Record 250 of 500 | sabang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sabang
    Processing Record 251 of 500 | simao dias
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=simao%20dias
    lemesos not found. Skipping...
    caborca not found. Skipping...
    Processing Record 252 of 500 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kalmunai
    Processing Record 253 of 500 | brigantine
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=brigantine
    Processing Record 254 of 500 | ares
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ares
    Processing Record 255 of 500 | bodden town
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bodden%20town
    Processing Record 256 of 500 | rodas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rodas
    Processing Record 257 of 500 | mecca
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mecca
    Processing Record 258 of 500 | sistranda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sistranda
    Processing Record 259 of 500 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=coquimbo
    Processing Record 260 of 500 | nanchital
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nanchital
    Processing Record 261 of 500 | quelimane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=quelimane
    Processing Record 262 of 500 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pangnirtung
    Processing Record 263 of 500 | mizdah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mizdah
    Processing Record 264 of 500 | kovdor
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kovdor
    Processing Record 265 of 500 | bloomingdale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bloomingdale
    Processing Record 266 of 500 | naze
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=naze
    Processing Record 267 of 500 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cidreira
    Processing Record 268 of 500 | parvatipuram
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=parvatipuram
    Processing Record 269 of 500 | breves
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=breves
    Processing Record 270 of 500 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=san%20patricio
    Processing Record 271 of 500 | boende
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=boende
    Processing Record 272 of 500 | kariapatti
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kariapatti
    Processing Record 273 of 500 | mandurah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mandurah
    Processing Record 274 of 500 | shimoda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=shimoda
    Processing Record 275 of 500 | martapura
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=martapura
    Processing Record 276 of 500 | talakan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=talakan
    Processing Record 277 of 500 | goundam
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=goundam
    Processing Record 278 of 500 | adre
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=adre
    Processing Record 279 of 500 | maceio
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=maceio
    Processing Record 280 of 500 | pasco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pasco
    Processing Record 281 of 500 | lesnoy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lesnoy
    tungkang not found. Skipping...
    Processing Record 282 of 500 | anuchino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=anuchino
    Processing Record 283 of 500 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fairbanks
    Processing Record 284 of 500 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=leningradskiy
    ekhabi not found. Skipping...
    olafsvik not found. Skipping...
    Processing Record 285 of 500 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ketchikan
    ruatoria not found. Skipping...
    Processing Record 286 of 500 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=esperance
    Processing Record 287 of 500 | ancud
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ancud
    Processing Record 288 of 500 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=san%20cristobal
    Processing Record 289 of 500 | brae
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=brae
    Processing Record 290 of 500 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yarmouth
    Processing Record 291 of 500 | becerril
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=becerril
    Processing Record 292 of 500 | sovetskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sovetskiy
    Processing Record 293 of 500 | hami
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hami
    Processing Record 294 of 500 | pereslavl-zalesskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pereslavl-zalesskiy
    Processing Record 295 of 500 | santa maria
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=santa%20maria
    Processing Record 296 of 500 | lata
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lata
    Processing Record 297 of 500 | parrita
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=parrita
    Processing Record 298 of 500 | berdigestyakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=berdigestyakh
    Processing Record 299 of 500 | koslan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=koslan
    Processing Record 300 of 500 | pimentel
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pimentel
    Processing Record 301 of 500 | tual
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tual
    Processing Record 302 of 500 | kasongo-lunda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kasongo-lunda
    Processing Record 303 of 500 | suleja
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=suleja
    barentsburg not found. Skipping...
    sentyabrskiy not found. Skipping...
    Processing Record 304 of 500 | warri
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=warri
    bolungarvik not found. Skipping...
    Processing Record 305 of 500 | orativ
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=orativ
    Processing Record 306 of 500 | zacatepec
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zacatepec
    dien bien not found. Skipping...
    Processing Record 307 of 500 | belmonte
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=belmonte
    Processing Record 308 of 500 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=belaya%20gora
    Processing Record 309 of 500 | cairns
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cairns
    jimma not found. Skipping...
    Processing Record 310 of 500 | biltine
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=biltine
    Processing Record 311 of 500 | charters towers
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=charters%20towers
    Processing Record 312 of 500 | kita
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kita
    Processing Record 313 of 500 | tizimin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tizimin
    Processing Record 314 of 500 | senanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=senanga
    Processing Record 315 of 500 | catuday
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=catuday
    Processing Record 316 of 500 | ca mau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ca%20mau
    Processing Record 317 of 500 | razole
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=razole
    Processing Record 318 of 500 | the pas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=the%20pas
    Processing Record 319 of 500 | kapustin yar
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kapustin%20yar
    pribelskiy not found. Skipping...
    Processing Record 320 of 500 | santa quiteria do maranhao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=santa%20quiteria%20do%20maranhao
    Processing Record 321 of 500 | nelson bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nelson%20bay
    mayor pablo lagerenza not found. Skipping...
    Processing Record 322 of 500 | marsabit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=marsabit
    Processing Record 323 of 500 | iberia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=iberia
    Processing Record 324 of 500 | malanje
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=malanje
    Processing Record 325 of 500 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rikitea
    Processing Record 326 of 500 | caranavi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=caranavi
    Processing Record 327 of 500 | sarankhola
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sarankhola
    mataura not found. Skipping...
    Processing Record 328 of 500 | qaqortoq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=qaqortoq
    Processing Record 329 of 500 | manavalakurichi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=manavalakurichi
    Processing Record 330 of 500 | enshi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=enshi
    Processing Record 331 of 500 | mananjary
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mananjary
    Processing Record 332 of 500 | rawson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rawson
    Processing Record 333 of 500 | visby
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=visby
    Processing Record 334 of 500 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=faanui
    Processing Record 335 of 500 | el tigre
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=el%20tigre
    tasbuget not found. Skipping...
    Processing Record 336 of 500 | luderitz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=luderitz
    Processing Record 337 of 500 | saint-joseph
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint-joseph
    Processing Record 338 of 500 | dunda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dunda
    Processing Record 339 of 500 | kibuye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kibuye
    Processing Record 340 of 500 | calbuco
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=calbuco
    Processing Record 341 of 500 | lebanon
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lebanon
    Processing Record 342 of 500 | carutapera
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=carutapera
    Processing Record 343 of 500 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fort%20nelson
    Processing Record 344 of 500 | mahajanga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mahajanga
    Processing Record 345 of 500 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint%20anthony
    Processing Record 346 of 500 | gamboula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=gamboula
    Processing Record 347 of 500 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=grand%20gaube
    Processing Record 348 of 500 | kurchum
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kurchum
    Processing Record 349 of 500 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=geraldton
    Processing Record 350 of 500 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=new%20norfolk
    Processing Record 351 of 500 | mastic beach
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mastic%20beach
    Processing Record 352 of 500 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bambous%20virieux
    Processing Record 353 of 500 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ribeira%20grande
    Processing Record 354 of 500 | dauriya
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dauriya
    Processing Record 355 of 500 | muhos
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=muhos
    Processing Record 356 of 500 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sao%20joao%20da%20barra
    Processing Record 357 of 500 | itaguai
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=itaguai
    Processing Record 358 of 500 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hasaki
    Processing Record 359 of 500 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=port%20lincoln
    Processing Record 360 of 500 | pavlivka
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pavlivka
    Processing Record 361 of 500 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=constitucion
    Processing Record 362 of 500 | east london
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=east%20london
    Processing Record 363 of 500 | jolalpan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=jolalpan
    afmadu not found. Skipping...
    Processing Record 364 of 500 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=souillac
    la uribe not found. Skipping...
    Processing Record 365 of 500 | libourne
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=libourne
    Processing Record 366 of 500 | jaciara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=jaciara
    Processing Record 367 of 500 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=iqaluit
    vaitupu not found. Skipping...
    sisian not found. Skipping...
    Processing Record 368 of 500 | ningxiang
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ningxiang
    Processing Record 369 of 500 | san vicente
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=san%20vicente
    Processing Record 370 of 500 | aloleng
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=aloleng
    Processing Record 371 of 500 | whitehorse
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=whitehorse
    Processing Record 372 of 500 | ust-uda
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ust-uda
    Processing Record 373 of 500 | ballina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ballina
    henties bay not found. Skipping...
    Processing Record 374 of 500 | wilmington
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=wilmington
    Processing Record 375 of 500 | ystad
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ystad
    Processing Record 376 of 500 | sao caetano de odivelas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sao%20caetano%20de%20odivelas
    Processing Record 377 of 500 | joshimath
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=joshimath
    Processing Record 378 of 500 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kruisfontein
    Processing Record 379 of 500 | banjarmasin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=banjarmasin
    Processing Record 380 of 500 | pervomaysk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pervomaysk
    yerraguntla not found. Skipping...
    Processing Record 381 of 500 | skelleftea
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=skelleftea
    Processing Record 382 of 500 | bud
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bud
    Processing Record 383 of 500 | kouqian
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kouqian
    Processing Record 384 of 500 | palmer
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=palmer
    Processing Record 385 of 500 | durusu
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=durusu
    Processing Record 386 of 500 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=half%20moon%20bay
    Processing Record 387 of 500 | yermakovskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=yermakovskoye
    ulagan not found. Skipping...
    karaul not found. Skipping...
    zachagansk not found. Skipping...
    Processing Record 388 of 500 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=cabo%20san%20lucas
    Processing Record 389 of 500 | vao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vao
    Processing Record 390 of 500 | manta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=manta
    Processing Record 391 of 500 | torez
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=torez
    Processing Record 392 of 500 | carlyle
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=carlyle
    Processing Record 393 of 500 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bathsheba
    Processing Record 394 of 500 | marinette
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=marinette
    saravan not found. Skipping...
    Processing Record 395 of 500 | kirakira
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kirakira
    Processing Record 396 of 500 | palora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=palora
    Processing Record 397 of 500 | partenit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=partenit
    semme not found. Skipping...
    nikolskoye not found. Skipping...
    Processing Record 398 of 500 | sorong
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=sorong
    Processing Record 399 of 500 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ushuaia
    Processing Record 400 of 500 | mosjoen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mosjoen
    Processing Record 401 of 500 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tiksi
    Processing Record 402 of 500 | tabou
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tabou
    Processing Record 403 of 500 | rancho veloz
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rancho%20veloz
    Processing Record 404 of 500 | shoranur
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=shoranur
    Processing Record 405 of 500 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mahebourg
    Processing Record 406 of 500 | kalety
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kalety
    Processing Record 407 of 500 | saint simons
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint%20simons
    Processing Record 408 of 500 | el alto
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=el%20alto
    Processing Record 409 of 500 | vostok
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vostok
    Processing Record 410 of 500 | antsohihy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=antsohihy
    tumannyy not found. Skipping...
    Processing Record 411 of 500 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=petropavlovsk-kamchatskiy
    Processing Record 412 of 500 | te anau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=te%20anau
    Processing Record 413 of 500 | camacha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=camacha
    Processing Record 414 of 500 | ishigaki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ishigaki
    Processing Record 415 of 500 | hofn
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hofn
    Processing Record 416 of 500 | maldonado
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=maldonado
    Processing Record 417 of 500 | tyumen
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=tyumen
    Processing Record 418 of 500 | kurumkan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kurumkan
    Processing Record 419 of 500 | seoul
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=seoul
    Processing Record 420 of 500 | xining
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=xining
    Processing Record 421 of 500 | chapais
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=chapais
    Processing Record 422 of 500 | talnakh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=talnakh
    Processing Record 423 of 500 | wad rawah
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=wad%20rawah
    Processing Record 424 of 500 | kidal
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kidal
    Processing Record 425 of 500 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint-augustin
    Processing Record 426 of 500 | bitung
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bitung
    Processing Record 427 of 500 | ulaangom
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ulaangom
    airai not found. Skipping...
    Processing Record 428 of 500 | bourail
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bourail
    Processing Record 429 of 500 | burgeo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=burgeo
    Processing Record 430 of 500 | thunder bay
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=thunder%20bay
    Processing Record 431 of 500 | jiaxing
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=jiaxing
    Processing Record 432 of 500 | amga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=amga
    Processing Record 433 of 500 | nome
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nome
    Processing Record 434 of 500 | waipawa
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=waipawa
    Processing Record 435 of 500 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=puerto%20ayora
    Processing Record 436 of 500 | liwale
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=liwale
    Processing Record 437 of 500 | bar harbor
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bar%20harbor
    Processing Record 438 of 500 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=okhotsk
    Processing Record 439 of 500 | nyurba
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nyurba
    Processing Record 440 of 500 | inhambane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=inhambane
    Processing Record 441 of 500 | bull savanna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bull%20savanna
    Processing Record 442 of 500 | fare
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=fare
    Processing Record 443 of 500 | penalva
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=penalva
    Processing Record 444 of 500 | marawi
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=marawi
    Processing Record 445 of 500 | nola
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nola
    Processing Record 446 of 500 | grindavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=grindavik
    Processing Record 447 of 500 | lokosovo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=lokosovo
    Processing Record 448 of 500 | kitgum
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kitgum
    Processing Record 449 of 500 | kimberley
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kimberley
    Processing Record 450 of 500 | araouane
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=araouane
    Processing Record 451 of 500 | panguna
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=panguna
    Processing Record 452 of 500 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nanortalik
    Processing Record 453 of 500 | kupino
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kupino
    Processing Record 454 of 500 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=komsomolskiy
    Processing Record 455 of 500 | ormara
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ormara
    Processing Record 456 of 500 | zelenets
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=zelenets
    Processing Record 457 of 500 | dalianwan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dalianwan
    Processing Record 458 of 500 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=narsaq
    Processing Record 459 of 500 | ulvik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=ulvik
    Processing Record 460 of 500 | krasnovishersk
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=krasnovishersk
    Processing Record 461 of 500 | kazachinskoye
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kazachinskoye
    Processing Record 462 of 500 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=arraial%20do%20cabo
    Processing Record 463 of 500 | pershotravneve
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=pershotravneve
    Processing Record 464 of 500 | baijiantan
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=baijiantan
    marzuq not found. Skipping...
    Processing Record 465 of 500 | saint-francois
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=saint-francois
    Processing Record 466 of 500 | mountain home
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mountain%20home
    Processing Record 467 of 500 | isiro
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=isiro
    muktagachha not found. Skipping...
    Processing Record 468 of 500 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hobart
    Processing Record 469 of 500 | kamina
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kamina
    Processing Record 470 of 500 | vohibinany
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vohibinany
    Processing Record 471 of 500 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=avarua
    Processing Record 472 of 500 | myski
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=myski
    urumqi not found. Skipping...
    Processing Record 473 of 500 | kapit
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kapit
    Processing Record 474 of 500 | payakumbuh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=payakumbuh
    cheuskiny not found. Skipping...
    Processing Record 475 of 500 | bima
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bima
    amderma not found. Skipping...
    Processing Record 476 of 500 | mwinilunga
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mwinilunga
    Processing Record 477 of 500 | beidao
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=beidao
    Processing Record 478 of 500 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=meulaboh
    Processing Record 479 of 500 | nushki
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=nushki
    Processing Record 480 of 500 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=bluff
    asfi not found. Skipping...
    Processing Record 481 of 500 | asau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=asau
    Processing Record 482 of 500 | gangawati
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=gangawati
    Processing Record 483 of 500 | volchikha
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=volchikha
    Processing Record 484 of 500 | visnes
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=visnes
    Processing Record 485 of 500 | huarmey
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=huarmey
    Processing Record 486 of 500 | kahului
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=kahului
    Processing Record 487 of 500 | dharchula
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=dharchula
    Processing Record 488 of 500 | balakhta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=balakhta
    Processing Record 489 of 500 | forestville
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=forestville
    Processing Record 490 of 500 | westport
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=westport
    Processing Record 491 of 500 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=hermanus
    Processing Record 492 of 500 | juneau
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=juneau
    Processing Record 493 of 500 | iki-burul
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=iki-burul
    tidore not found. Skipping...
    itarema not found. Skipping...
    Processing Record 494 of 500 | vizianagaram
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=vizianagaram
    Processing Record 495 of 500 | rosetta
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=rosetta
    Processing Record 496 of 500 | mayo
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mayo
    Processing Record 497 of 500 | mantua
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=mantua
    Processing Record 498 of 500 | husavik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=husavik
    Processing Record 499 of 500 | esik
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=esik
    Processing Record 500 of 500 | barcelona
    http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=90ae716c8d520ec3abade149f9b5ba47&q=barcelona
    -----------------------------
    Data Retrieval Complete
    -----------------------------



```python
# Create df from weather data
weather_df = pd.DataFrame(weather_data)

# Check that there are at least 500 unique cities obtained from API requests
len(weather_df['City'].unique())
```




    500




```python
# Peek at data
weather_df.head()
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
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max_Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nhulunbuy</td>
      <td>40</td>
      <td>AU</td>
      <td>1512345600</td>
      <td>59</td>
      <td>-12.23</td>
      <td>136.77</td>
      <td>89.60</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Kattivakkam</td>
      <td>40</td>
      <td>IN</td>
      <td>1512343800</td>
      <td>88</td>
      <td>13.22</td>
      <td>80.32</td>
      <td>77.00</td>
      <td>7.63</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ciudad Bolivar</td>
      <td>88</td>
      <td>VE</td>
      <td>1512347501</td>
      <td>85</td>
      <td>8.12</td>
      <td>-63.55</td>
      <td>79.47</td>
      <td>12.77</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Kavaratti</td>
      <td>68</td>
      <td>IN</td>
      <td>1512347500</td>
      <td>100</td>
      <td>10.57</td>
      <td>72.64</td>
      <td>80.91</td>
      <td>22.28</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ampanihy</td>
      <td>0</td>
      <td>MG</td>
      <td>1512347504</td>
      <td>93</td>
      <td>-24.70</td>
      <td>44.75</td>
      <td>67.95</td>
      <td>2.93</td>
    </tr>
  </tbody>
</table>
</div>



## Latitude vs. Temperature Plot


```python
plt.scatter(weather_df['Lat'], weather_df['Max_Temp'],
           marker = 'o', facecolors='blue', 
           edgecolors='black')
plt.xlabel('Latitude')
plt.ylabel('Max Temperature (F)')
plt.title('City Latitude vs. Max Temperature (12/02/17)')
plt.grid(True)
plt.savefig('CityLatvsMaxTemp.png')
plt.show()
```


![png](output_12_0.png)


## Latitude vs. Humidity Plot


```python
plt.scatter(weather_df['Lat'], weather_df['Humidity'],
           marker = 'o', facecolors='blue', 
           edgecolors='black')
plt.xlabel('Latitude')
plt.ylabel('Humidity (%)')
plt.title('City Latitude vs. Humidity (12/02/17)')
plt.xlim(-80, 100)
plt.ylim(-20, 120)
plt.grid(True)
plt.savefig('CityLatvsHumidity.png')
plt.show()
```


![png](output_14_0.png)


## Latitude vs. Cloudiness Plot


```python
plt.scatter(weather_df['Lat'], weather_df['Cloudiness'],
           marker = 'o', facecolors='blue', 
           edgecolors='black', alpha=0.8)
plt.xlabel('Latitude')
plt.ylabel('Cloudiness (%)')
plt.title('City Latitude vs. Cloudiness (12/02/17)')
plt.xlim(-80, 100)
plt.ylim(-20, 120)
plt.grid(True)
plt.savefig('CityLatvsCloudiness.png')
plt.show()
```


![png](output_16_0.png)


## Latitude vs. Wind Speed Plot


```python
plt.scatter(weather_df['Lat'], weather_df['Wind Speed'],
           marker = 'o', facecolors='blue', 
           edgecolors='black', alpha=0.8)
plt.xlabel('Latitude')
plt.ylabel('Wind Speed (mph)')
plt.title('City Latitude vs. Wind Speed (12/02/17)')
plt.xlim(-80, 100)
plt.ylim(-5, 40)
plt.grid(True)
plt.savefig('CityLatvsWindSpeed.png')
plt.show()
```


![png](output_18_0.png)



```python
# Save df as csv
weather_df.to_csv('MG_weather_df.csv', index=False)
```

## Analysis

### - One of the evident trends is that near latitude 0 (the equator), cities have a higher temperature.
### - Following up on the first point, humidity percentage is highest in between -20 and 0 degrees latitude.
### - Cities towards higher latitudes seem to have higher wind speeds. Especially between 50 and 80 degrees latitude.


```python

```
