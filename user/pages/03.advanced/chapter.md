---
title: CO2, VTOC
taxonomy:
    category: docs
---

##Rozszerzenie funkcjonalności stacji o sensor mierzący CO2 oraz TVOC dla Raspberry Pi

###Lista niezbędnych komponentów:
1. Sensor CCS811 mierzący CO2 oraz TVOC, dostępny na [allegro.pl](http://bit.ly/2kitahS), w polskich sklepach z elektroniką czy aliexpress
2. Stację pomiarową zbudowaną z działu poprzedniego [Temp/Wilg/Ciśn](http://lintech.hekko24.pl/intermediate)



###Schemat połączenia CCS811 z Raspberry Pi 0 W

![Schemat połączenia autorstwa adafruit.com](https://cdn-learn.adafruit.com/assets/assets/000/045/239/medium640/adafruit_products_CCS811_pi_bb.png)

+ Port Vin CCS811 do 3V RPi3W 
+ Port GND do GND RPi3W
+ Port SDA do SDA do SDA RPi3W
+ Port SCL do SCL do SCL RPi3W
+ Port Wake do GND RPi3W


###Instalacja

```bash
echo "dtparam=i2c_baudrate=10000" >> /boot/config.txt
cd /etc/configuration
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/CCS811_RPi.py
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/ccs811.py
chmod +x /etc/configuration/*.py

(crontab -l 2>/dev/null; echo "*/5 * * * * /etc/configuration/ccs811.py") | crontab -
```

>>>>>Skrypt pytona wykorzystuje wcześniej pobrany plik (configuration.data) z GitHub'a (patrz dział [AirMonitor](http://lintech.hekko24.pl/basics/installation)) oraz sensor BME280 dla kompensacji pomiarów CO2 i TVOC.
