---
title: Oczyść powietrze
taxonomy:
    category: docs
---

##Rozszerzenie funkcjonalności stacji o zarządzanie filtrem powietrza w oparciu o system inteligentnego domu Domoticz.

>>>Pewnie zastanawiasz się dlaczego sterowanie filtrem powietrza ma się zająć Domoticz skoro Xiaomi Air Purifier posiada tryb auto dostosowujący obroty do stężenia pyłów w powietrzu? Odpowiedź jest prosta, po pierwsze sensor powietrza w filtrze Xiaomi nie jest zbyt dokładny, po drugie zauważyłem, że pomimo dużego stężenia pyłów w mieszkaniu filtr Xiaomi nie chciał przyśpieszyć swoich obrotów aby pozbyć się zanieczyszczeń. Natomiast dzięki sterowaniu za pomocą Domoticza w oparciu o zewnętrzny, laserowy sensor filtr powietrza Xiaomi jest sterowany w dokładniejszy i bardziej elastyczny sposób.

###Lista niezbędnych komponentów:
1. Filtr powietrza Xiaomi AirPurifier2 [allegro.pl](https://allegro.pl/listing?string=Xiaomi%20Air%20Purifier%202&order=m&bmatch=ss-base-relevance-floki-5-nga-hcp-ele-1-4-1003), [zwinnemiasto.pl](http://zwinnemiasto.pl/hulajnogi/oczyszczacz-powietrza-xiaomi/), [lifeplanet.pl](https://lifeplanet.pl/kategoria/oczyszczacze-powietrza/xiaomi-air-purifier-2-inteligentny-oczyszczacz-powietrza) czy aliexpress lub gearbest
![Xiaomi Air Purifier 2](http://airmonitor.pl/images/image27.jpg)

2. Stację pomiarową zbudowaną z działu poprzedniego [Pyły PM1, PM2,5, PM10](http://airmonitor.pl/basics/overview)

###Instalacja

####Domoticz

```bash
sudo apt-get install htop dos2unix vim apt-show-versions mailutils exfat-fuse exfat-utils screen curl wiringpi i2c-tools aptitude watchdog zlib1g-dev -y
sudo apt-get install cmake make gcc g++ libssl-dev git libcurl4-openssl-dev libusb-dev libudev-dev libffi-dev python3.5 python3.5-dev python3-pip -y
sudo apt-get remove libboost-dev libboost-thread-dev libboost-system-dev libboost-atomic-dev libboost-regex-dev libboost-date-time1.62-dev libboost-date-time1.62.0 libboost-atomic1.62.0 libboost-regex1.62.0 libboost-iostreams1.62.0 libboost-serialization1.62-dev libboost-serialization1.62.0 libboost-system1.62-dev libboost-system1.62.0 libboost-thread1.62-dev libboost-thread1.62.0 libboost1.62-dev build-essential tk-dev libncurses5-dev libncursesw5-dev libreadline6-dev libdb5.3-dev libgdbm-dev libsqlite3-dev libssl-dev libbz2-dev libexpat1-dev liblzma-dev -y
sudo apt-get autoremove
cp /usr/share/zoneinfo/Europe/Warsaw /etc/localtime

mkdir -p /home/pi/domoticz
cd /home/pi/domoticz/
git clone https://github.com/OpenZWave/open-zwave open-zwave-read-only
cd open-zwave-read-only
make
cd ..

mkdir boost
cd boost
wget https://dl.bintray.com/boostorg/release/1.66.0/source/boost_1_66_0.tar.gz
tar xfz boost_1_66_0.tar.gz
rm boost_1_66_0.tar.gz
cd boost_1_66_0/
./bootstrap.sh
./b2 stage threading=multi link=static --with-thread --with-date_time --with-system --with-atomic --with-regex
sudo ./b2 install threading=multi link=static --with-thread --with-date_time --with-system --with-atomic --with-regex
cd ../../
rm -Rf boost/

git clone -b development https://github.com/domoticz/domoticz.git domoticz
cd domoticz
cmake -DBOOST_LIBRARYDIR=/usr/lib/x86_64-linux-gnu -DCMAKE_BUILD_TYPE=Release -DBoost_USE_MULTITHREADED=OFF
cmake -USE_STATIC_OPENZWAVE -DCMAKE_BUILD_TYPE=Release CMakeLists.txt
sudo fallocate -l 1G /tmp/tmpswap
sudo mkswap /tmp/tmpswap
sudo swapon /tmp/tmpswap
CONF_SWAPFILE=1024m
make
sudo cp domoticz.sh /etc/init.d/
sudo chmod +x /etc/init.d/domoticz.sh
sudo update-rc.d domoticz.sh defaults

sudo vi /etc/init.d/domoticz.sh
USERNAME=pi
DAEMON=/home/$USERNAME/domoticz/domoticz/$NAME
DAEMON_ARGS="$DAEMON_ARGS -www 8080"
DAEMON_ARGS="$DAEMON_ARGS -sslwww 443"
DAEMON_ARGS="$DAEMON_ARGS -log /tmp/domoticz.txt"
DAEMON_ARGS="$DAEMON_ARGS -syslog"
```



####Xiaomi Air Purifier

```bash
cd /home/pi/domoticz/domoticz/plugins
git clone https://github.com/kofec/domoticz-AirPurifier
chmod +x domoticz-AirPurifier/*
```

>>>>>Dodaj filtr do domoticza korzystając z [opisu](https://github.com/kofec/domoticz-AirPurifier)

###Konfiguracja systemu Domoticz
1. Utwórz wirtualne, niestandardowe urządzenie o nazwie SDS021 bądź PMS7003 w zależności, który sensor wykorzystujesz. Możesz oczywiście uzyć innej nazwy :)
2. Przy uzyciu bash'a bądź pythona przesyłaj dane z sensora do systemu domoticz. Poniżej przykład komendy w języku python:


####Wysyłanie danych ze stacji opartej na Raspberry Pi 0 W
```python
import urllib.request
from configparser import ConfigParser
parser = ConfigParser()
parser.read('/etc/configuration/configuration.data')
domoticz_ip_address=(parser.get('domoticz', 'domoticz_ip_address'))
domoticz_port=(parser.get('domoticz', 'domoticz_port'))
inside_ppm01_idx = (parser.get('domoticz', 'inside_ppm01_idx'))
inside_ppm10_idx = (parser.get('domoticz', 'inside_ppm10_idx'))
inside_ppm25_idx = (parser.get('domoticz', 'inside_ppm25_idx'))
pm1 = (open('/mnt/ramdisk_ram0/ppm1').read())
pm1 = float(pm1)
print(pm1)
pm25 = (open('/mnt/ramdisk_ram0/ppm25').read())
pm25 = float(pm25)
print(pm25)
pm10 = (open('/mnt/ramdisk_ram0/ppm10').read())
pm10 = float(pm10)
print(pm10)
TEMP_PRESSURE_HUMIDITY = urllib.request.urlopen("http://" + str(domoticz_ip_address) + ":" + str(domoticz_port) + "/json.htm?type=command&param=udevice&idx=" + str(temp_hum_pressure_sensor_0_idx) + "&nvalue=0&svalue=" + str(temp_out) + ";" + str(humidity_out) + ";0;" + str(pressure_out_hPa) + ";0")
PPM25_INSIDE = urllib.request.urlopen("http://" + str(domoticz_ip_address) + ":" + str(domoticz_port) + "/json.htm?type=command&param=udevice&idx=" + str(inside_ppm25_idx) + "&nvalue=0&svalue=" + str(pm25), timeout=10)
PPM10_INSIDE = urllib.request.urlopen("http://" + str(domoticz_ip_address) + ":" + str(domoticz_port) + "/json.htm?type=command&param=udevice&idx=" + str(inside_ppm10_idx) + "&nvalue=0&svalue=" + str(pm10), timeout=10)
PPM01_INSIDE = urllib.request.urlopen("http://" + str(domoticz_ip_address) + ":" + str(domoticz_port) + "/json.htm?type=command&param=udevice&idx=" + str(inside_ppm01_idx) + "&nvalue=0&svalue=" + str(pm1), timeout=10)
print(PPM10_INSIDE.read())
print(PPM25_INSIDE.read())
print(PPM01_INSIDE.read())
print(TEMP_PRESSURE_HUMIDITY.read())
```

3. Następnie utwórz kilka virtualnych urządzeń - przełączników o następujących nazwach:
* AirPurifier_ON_OFF
* AirPurifier_Auto
* AirPurifier_Silent
* AirPurifier_Low
* AirPurifier_Medium
* AirPurifier_Max
* CO2

4. Kolejny krok to ręczne utworzenie eventu czy to w typie LUA bądź Blocky z regułami mapującymi tryb działania filtra powietrza w oparciu o wskazania zewnętrznego sensora SDS lub PMS.

>>>>>Tekst ten powstał w oparciu o materiały zebrane z [forum domoticza](http://www.domoticz.com/forum/viewtopic.php?t=15537)



