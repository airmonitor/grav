---
title: API
taxonomy:
    category: docs
---

### API

>>>>Pamiętaj aby podmienić długość oraz szerokość geograficzną w pliku **/etc/configuration/configuration.data** oraz **wypełnić [formularz](https://docs.google.com/forms/d/e/1FAIpQLSdw72_DggyrK7xnSQ1nR11Y-YK4FYWk_MF9QbecpOERql-T2w/viewform)** jeśli chcesz aby upublicznić Twoje pomiary

Dostępne jest również REST API o adresie **http://api.airmonitor.pl:5000/api**

###Przykład dla python'a 3.x:

####Pyły zawieszone:


#####Dla [SDS021](http://allegro.pl/show_item.php?item=6898587945)/011:
Python:
```python
import json
import requests

data = '{"lat": "' + str(55.000) + '", ' \
        '"long": "'+ str(15.000) + '", ' \
        '"pm25": ' + str(float(25)) + ', ' \
        '"pm10":' + str(float(10)) + ', ' \
        '"sensor": "SDS021"}'
url = 'http://api.airmonitor.pl:5000/api'
resp = requests.post(url, timeout=10, data=json.dumps(data), headers={"Content-Type": "application/json"})
resp.status_code
```

Bash:
```bash
curl -X POST -H "Content-Type: application/json" -d '{
            "lat": "55.000",
            "long": "15.000",
            "pm25": 25,
            "pm10": 10,
            "sensor": "SDS021"
}' http://api.airmonitor.pl:5000/api
```

NodeJS:

npm install --save request

```text
const request = require('request');

let data = {
  lat: "55.000",
  long: "15.000",
  pm25: 25,
  pm10: 10,
  sensor: "SDS021"
};

let options = {
  uri: "http://api.airmonitor.pl:5000/api",
  method: "POST",
  json: data
};

request(options, (error, response) => {
  if (error) {
    return console.log(error);
  }
  if (response.statusCode === 200) {
    // Success
  }
});
```


#####Dla [PMS7003](http://allegro.pl/show_item.php?item=7097428244)/5003:
Python:
```python
import json
import requests
data = '{"lat": "' + str(55.000) + '", ' \
        '"long": "'+ str(15.000) + '", ' \
        '"pm1": ' + str(float(1)) + ', ' \
        '"pm25": ' + str(float(25)) + ', ' \
        '"pm10":' + str(float(10)) + ', ' \
        '"sensor": "PMS7003"}'
url = ('http://api.airmonitor.pl:5000/api')
resp = requests.post(url, timeout=10, data=json.dumps(data), headers={"Content-Type": "application/json"})
resp.status_code
```

Bash:
```bash
curl -X POST -H "Content-Type: application/json" -d '{
            "lat": "55.000",
            "long": "15.000",
            "pm1": 1,
            "pm25": 25,
            "pm10": 10,
            "sensor": "PMS7003"
}' http://api.airmonitor.pl:5000/api
```

NodeJS*:

```text
const request = require('request');

let data = {
  lat: "55.000",
  long: "15.000",
  pm1: 1,
  pm25: 25,
  pm10: 10,
  sensor: "PMS7003"
};

let options = {
  uri: "http://api.airmonitor.pl:5000/api",
  method: "POST",
  json: data
};

request(options, (error, response) => {
  if (error) {
    return console.log(error);
  }
  if (response.statusCode === 200) {
    // Success
  }
});
```

Jak widać powyżej, jedyną różnicą między wersją dla sensorów SDS a PMS jest dodatkowa linia wysyłająca wartość pyłu zawieszonego PM1.

####Temperatura, wilgotność, ciśnienie:

#####Dla BME280:
Python:
```python
import json
import requests
data = '{"lat": "' + str(55.000) + '", ' \
        '"long": "'+ str(15.000) + '", ' \
        '"pressure": ' + str(float(1000)) + ', ' \
        '"temperature": ' + str(float(25)) + ', ' \
        '"humidity": ' + str(float(55)) + ', ' \
        '"sensor": "BME280"}'
url = 'http://api.airmonitor.pl:5000/api'
resp = requests.post(url, timeout=10, data=json.dumps(data), headers={"Content-Type": "application/json"})
resp.status_code
```

Bash:
```bash
curl -X POST -H "Content-Type: application/json" -d '{
            "lat": "55.000",
            "long": "15.000",
            "pressure": 1000.10,
            "temperature": 25.2,
            "humidity": 55,
            "sensor": "BME280"
}' http://api.airmonitor.pl:5000/api
```

NodeJS*:

```text
const request = require('request');

let data = {
  lat: "55.000",
  long: "15.000",
  pressure: 1000.10,
  temperature: 25.2,
  humidity: 55,
  sensor: "BME280"
};

let options = {
  uri: "http://api.airmonitor.pl:5000/api",
  method: "POST",
  json: data
};

request(options, (error, response) => {
  if (error) {
    return console.log(error);
  }
  if (response.statusCode === 200) {
    // Success
  }
});
```

####CO2, TVOC:

#####Dla [CCS811](http://allegro.pl/ccs811-czujnik-co2-czasteczek-organicznych-i6961041870.html):
Python:
```python
import json
import requests
data = '{"lat": "' + str(55.000) + '", ' \
        '"long": "'+ str(15.000) + '", ' \
        '"co2": ' + str(float(400)) + ', ' \
        '"tvoc":' + str(float(0.1)) + ', ' \
        '"sensor": "CCS811"}'
        
url = 'http://api.airmonitor.pl:5000/api'
resp = requests.post(url, timeout=10, data=json.dumps(data), headers={"Content-Type": "application/json"})
resp.status_code
```

Bash:
```bash
curl -X POST -H "Content-Type: application/json" -d '{
            "lat": "55.000",
            "long": "15.000",
            "co2": 400.0,
            "tvoc": 0.1,
            "sensor": "CCS811"
}' http://api.airmonitor.pl:5000/api
```

NodeJS*:

```text
const request = require('request');

let data = {
  lat: "55.000",
  long: "15.000",
  co2: 400.0,
  tvoc: 0.1,
  sensor: "CCS811"
};

let options = {
  uri: "http://api.airmonitor.pl:5000/api",
  method: "POST",
  json: data
};

request(options, (error, response) => {
  if (error) {
    return console.log(error);
  }
  if (response.statusCode === 200) {
    // Success
  }
});
```

>>>>>NodeJS wymaga biblioteki request, instalacja - **npm install --save request**