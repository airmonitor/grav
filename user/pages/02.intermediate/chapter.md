---
title: Temp/Wilg/Ciśn
taxonomy:
    category: docs
---
##Rozszerzenie funkcjonalności stacji o sensor mierzący temperaturę, wilgotność oraz ciśnienie dla Raspberry Pi

###Lista niezbędnych komponentów:
1. BME280, dostępne na [allegro.pl](http://bit.ly/2BQddpR), polskich sklepach z elektroniką czy aliexpress
2. Stację pomiarową zbudowaną z działu poprzedniego [Pyły PM1, PM2,5, PM10](http://lintech.hekko24.pl/basics/overview)



###Schemat połączenia BME280 z Raspberry Pi 0 W

![Schemat wyprowadzenia przewodów dla BME280](http://airmonitor.pl/images/image21.jpg)

Przykład połączenia [wtyku kątowego 4-pinowego](https://botland.com.pl/zlacza-raster-254mm/6789-zlacze-raster-254mm-wtyk-katowy-4-pinowy-5szt.html?search_query=Zlacze+raster+2%2C54mm+-+wtyk+katowy+4-pinowy&results=7) oraz Raspberry Pi 0 W


![BME280 z wtykiem kątowym](http://airmonitor.pl/images/image13.jpg) ![BME280 z wtykiem kątowym](http://airmonitor.pl/images/image3.jpg)


###Instalacja

```bash
cd /etc/configuration
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/bme280.py
chmod +x /etc/configuration/*.py
(crontab -l 2>/dev/null; echo "*/5 * * * * /etc/configuration/bme280.py") | crontab -
```

>>>>>Skrypt pytona wykorzystuje wcześniej pobrany plik (configuration.data) z GitHub'a (patrz dział [Pyły PM1, PM2,5, PM10](http://lintech.hekko24.pl/basics/requirements)).
