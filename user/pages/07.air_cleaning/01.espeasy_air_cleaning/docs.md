---
title: EspEasy Mega
taxonomy:
    category: docs
---
####Wysyłanie danych ze stacji opartej o EspEasy Mega
#####Dla SDS0/11/18/21

Pamiętaj aby podmienić **DOMOTICZ_IP** oraz **IDX**

```
On SDS021#PM25 do
  SendToHTTP <DOMOTICZ_IP>,8080,/json.htm?type=command&param=udevice&idx=IDX&nvalue=0&svalue=[SDS021#PM25]
  SendToHTTP <DOMOTICZ_IP>,8080,/json.htm?type=command&param=udevice&idx=IDX&nvalue=0&svalue=[SDS021#PM10]
endon
```
![konfiguracja_espeasy_rules_sds](http://airmonitor.pl/images/espeasy_rules_sds021.jpg)


#####Dla MH-Z19
```
On CO2#PPM do
  SendToHTTP <DOMOTICZ_IP>,8080,/json.htm?type=command&param=udevice&idx=IDX&nvalue=0&svalue=[CO2#PPM]
endon
```
![konfiguracja_espeasy_rules_mh-z19](http://airmonitor.pl/images/espeasy_rules_mh-z19.jpg)



