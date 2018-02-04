---
title: Instalacja
taxonomy:
    category: docs
---

##Instalacja automatyczna
Celem skrócenia czasu potrzebnego na instalację oraz konfigurację systemu wykorzystamy [PiBakery](http://www.pibakery.org/download.html)

>>>>>PiBakery umożliwia automatyczną instalację systemu oraz późniejszą konfigurację. Dzięki temu po zakończonym procesie instalacji oraz konfiguracji uzyskasz w pełni działający sensor rejestrujący oraz wysyłający zmierzone wartości do serwisu airmonitor.pl

1. Pobieramy [przepis](https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/recipe_short.xml), który następnie należy zaimportować w PiBakery
![Import przepisu w PiBakery](http://airmonitor.pl/images/image11.jpg)

2. Zmień SSID sieci wifi (pi) oraz klucz dostępu (h0m3a1rm0n1t0r!)
![Zmiana SSID wifi oraz hasła](http://airmonitor.pl/images/image5.jpg)

3. Zapisz obraz ( dla Raspberry Pi 0 W sugeruję obraz lite )
![Zapis przepisu na karcie microsd](http://airmonitor.pl/images/image8.jpg)

4. Utwórz plik tekstowy o nazwie airmonitor.txt na karcie microsd i umieść w nim
![Zrzut ekranu prezentujący partycję boot z plikiem airmonitor.txt](http://airmonitor.pl/images/image6.jpg)
- pierwsza linia - szerokość geograficzna (latitude) umiejscowienia sensora
- druga linia - długość geograficzna (longitude) umiejscowienia sensora.

>>>>>>Szerokość jak i długość geograficzną możesz znaleźć korzystając http://mapa.airmonitor.pl. Kliknij w miejsce na mapie a w dymku znajdziesz szerokość oraz długość geograficzną.

Przykład prawidłowo wypełnionego pliku airmonitor.txt, **pamiętaj aby umieścić dane geograficzne umiejscowienia sensora!:**
![Plik airmonitor.txt](http://airmonitor.pl/images/image18.jpg)

5. Wyciągnij kartę microsd z czytnika i umieść ją w Raspberry Pi, podłącz zasilanie.

>>>>>Teraz nastąpi proces instalacji oraz konfiguracji systemu oraz sensora. Wszystkie zautomatyzowane czynności powinny zakończyć się w przeciągu godziny. Po automatycznym restarcie sensor zacznie wysyłać dane.

6. Wypełnij [formularz](https://docs.google.com/forms/d/e/1FAIpQLSdw72_DggyrK7xnSQ1nR11Y-YK4FYWk_MF9QbecpOERql-T2w/viewform) wykorzystując wcześniej zapisane wartości szerokości i długości geograficznej. Bez wypełnienia formularza mapa nie zostanie zaktualizowana o Twój sensor. W przeciągu kilku godzin zauważysz na mapie uruchomiony przez Ciebie sensor oznaczony pinezką.

>>>>>**Źródła wszystkich skryptów znajdziesz na [GitHub](https://github.com/airmonitor/home_air_monitor)**


##Instalacja manualna dla sensorów SDS*
```js
echo "dtoverlay=pi3-disable-bt" >> /boot/config.txt
echo "dtparam=i2c_arm=on" >> /boot/config.txt
echo "enable_uart=1" >> /boot/config.txt
sed  -i 's/fsck.repair=yes/fsck.repair=yes fsck.mode=force/g' /boot/cmdline.txt
sed  -i 's/console=serial0,115200 //g' /boot/cmdline.txt
echo "i2c-dev" >> /etc/modules

apt-get update
apt-get upgrade -y
apt-get install htop apt-mark dos2unix python3 python3-pip python3-serial python3-pip vim apt-show-versions mailutils exfat-fuse exfat-utils screen curl wiringpi i2c-tools aptitude watchdog

pip3 install -U pip
pip3 install -U setuptools
pip3 install -U pyserial
pip3 install -U requests
pip3 install -U RPi.bme280
pip3 install -U influxdb
pip3 install -U configparser
pip3 install -U urllib3


mkdir /etc/configuration
mkdir /mnt/ramdisk_ram0
cd /etc/configuration
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/configuration.data
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/sds011.py
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/smog_monitor.py
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/OTA.sh
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/on_reboot.sh


chmod +x /etc/configuration/*.py
chmod +x /etc/configuration/*.sh
echo "tmpfs                   /mnt/ramdisk_ram0       tmpfs   nodev,nosuid,size=1M 0 0" >> /etc/fstab

sed -i 's/#watchdog-device/watchdog-device/g' /etc/watchdog.conf
sed -i 's/#max-load-1/max-load-1/g' /etc/watchdog.conf
echo "watchdog-timeout=15" >> /etc/watchdog.conf
echo "bcm2835_wdt" >> /etc/modules
echo "WantedBy=multi-user.target" >> /lib/systemd/system/watchdog.service
echo "kernel.panic = 10" >> /etc/sysctl.conf
systemctl enable watchdog

(crontab -l 2>/dev/null; echo "*/5 * * * * /etc/configuration/smog_monitor.py") | crontab -
(crontab -l 2>/dev/null; echo "@reboot /etc/configuration/on_reboot.sh") | crontab -
(crontab -l 2>/dev/null; echo "59 23 * * * /etc/configuration/OTA.sh") | crontab -
```


###Instalacja manialna dla sensorów PMS*
```js
echo "dtoverlay=pi3-disable-bt" >> /boot/config.txt
echo "dtparam=i2c_arm=on" >> /boot/config.txt
echo "enable_uart=1" >> /boot/config.txt
sed  -i 's/fsck.repair=yes/fsck.repair=yes fsck.mode=force/g' /boot/cmdline.txt
sed  -i 's/console=serial0,115200 //g' /boot/cmdline.txt
echo "i2c-dev" >> /etc/modules

apt-get update
apt-get upgrade -y
apt-get install htop apt-mark dos2unix python3 python3-pip python3-serial python3-pip vim apt-show-versions mailutils exfat-fuse exfat-utils screen curl wiringpi i2c-tools aptitude watchdog

pip3 install -U pip
pip3 install -U setuptools
pip3 install -U pyserial
pip3 install -U requests
pip3 install -U RPi.bme280
pip3 install -U influxdb
pip3 install -U configparser
pip3 install -U urllib3


mkdir /etc/configuration
mkdir /mnt/ramdisk_ram0
cd /etc/configuration
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/configuration.data
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/running-pms.py
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/OTA.sh
wget https://raw.githubusercontent.com/airmonitor/home_air_monitor/master/on_reboot.sh


chmod +x /etc/configuration/*.py
chmod +x /etc/configuration/*.sh
echo "tmpfs                   /mnt/ramdisk_ram0       tmpfs   nodev,nosuid,size=1M 0 0" >> /etc/fstab

sed -i 's/#watchdog-device/watchdog-device/g' /etc/watchdog.conf
sed -i 's/#max-load-1/max-load-1/g' /etc/watchdog.conf
echo "watchdog-timeout=15" >> /etc/watchdog.conf
echo "bcm2835_wdt" >> /etc/modules
echo "WantedBy=multi-user.target" >> /lib/systemd/system/watchdog.service
echo "kernel.panic = 10" >> /etc/sysctl.conf
systemctl enable watchdog

(crontab -l 2>/dev/null; echo "*/5 * * * * /etc/configuration/running-pms.py") | crontab -
(crontab -l 2>/dev/null; echo "@reboot /etc/configuration/on_reboot.sh") | crontab -
(crontab -l 2>/dev/null; echo "59 23 * * * /etc/configuration/OTA.sh") | crontab -
```


>>>>Pamiętaj aby podmienić długość oraz szerokość geograficzną w pliku **/etc/configuration/configuration.data** oraz **wypełnić [formularz](https://docs.google.com/forms/d/e/1FAIpQLSdw72_DggyrK7xnSQ1nR11Y-YK4FYWk_MF9QbecpOERql-T2w/viewform)** jeśli chcesz aby upublicznić Twoje pomiary

###Konfiguracja EspEasy Mega (mega-20180126)

1. Zmień nazwę jednostki wg wzoru **szerokość geograficzna_długość geograficzna_SDSESP**. Przykład na poniższej ilustracji:
![Zrzut ekranu prezentujący konfigurację](http://airmonitor.pl/images/image30.jpg)

Po restarcie ekran startowy powinien wyświetlać nową nazwę:
![Zrzut ekranu prezentujący stronę główną](http://airmonitor.pl/images/image28.jpg)

2. Przejdź do zakładki **Controllers** a następnie skonfiguruj usługę OpenHAB MQTT wypełniając pola wg poniższego zrzutu ekranu:
![Zrzut ekranu prezentujący zakładkę controllers](http://airmonitor.pl/images/image31.jpg)

3. W zakładce **Devices** dodaj sensor Nova SDS011/018/198/021 postępując według poniżej ilustracji:
![Zrzut ekranu prezentujący zakładkę devices](http://airmonitor.pl/images/image29.jpg)

