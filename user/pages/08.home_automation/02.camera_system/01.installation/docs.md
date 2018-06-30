---
title: Instalacja
taxonomy:
    category: docs
---


###Alternatywny firmware dla Xiaofang:
1. Pobierz aktualny [obraz](https://github.com/samtap/fang-hacks/releases) i zapisz go na karcie pamięci
2. Podłącz kamerę do swojej sieci wifi wykorzystując aplikację mobilną MiHome i *pomiń aktualizację firmware'u*
3. Zmodyfikuj ustawienia kamery wykorzystując [instrukcję](https://github.com/samtap/fang-hacks)

###Alternatywny firmware dla Dafang:
1. Zestaw połączenie wifi wykorzystując aplikację Xiaomi MiHome. Potwierdzisz w ten sposób, że kamera na pewno działa bez jakichkolwiek modyfikacji.
2. Wykorzystując instrukcję oraz firmware z [repozytorim github](https://github.com/EliasKotlyar/Xiaomi-Dafang-Hacks) zainstaluj nowy firmware

###Instalacja i konfiguracja ZoneMinder'a (wersja dla Ubuntu)
4. Zainstaluj zoneminder wykorzystując opis poniżej:

```bash
sudo su
tasksel install lamp-server
add-apt-repository ppa:iconnor/zoneminder-master
apt-get update
apt-get upgrade
apt-get dist-upgrade
apt-get install zoneminder
vi /etc/systemd/system/multi-user.target.wants/apache2.service
```
Linię **PrivateTmp=True** zmienić na  **PrivateTmp=False**

**Konfiguracja MySQL:**

```bash
rm /etc/mysql/my.cnf
cp /etc/mysql/mysql.conf.d/mysqld.cnf /etc/mysql/my.cnf
nano /etc/mysql/my.cnf
```


do sekcji **[mysqld]** dodaj:

```sql_mode = NO_ENGINE_SUBSTITUTION```

```
systemctl restart mysql
mysql -uroot -p < /usr/share/zoneminder/db/zm_create.sql
mysql -uroot -p -e "grant select,insert,update,delete,create,alter,index,lock tables on zm.* to 'zmuser'@localhost identified by 'zmpass';"


chmod 740 /etc/zm/zm.conf
chown root:www-data /etc/zm/zm.conf
chown -R www-data:www-data /usr/share/zoneminder/

a2enconf zoneminder
a2enmod cgi
a2enmod rewrite

systemctl enable zoneminder
systemctl start zoneminder

nano /etc/php/7.1/apache2/php.ini
```


do sekcji **[Date]** dodaj:

```date.timezone = Europe/Warsaw```

```bash
systemctl reload apache2
```

ZoneMinder powinien być zainstalowany oraz nasłuchować. Uruchom przeglądarkę i przejdź pod adres http://**ZoneMinder IP**/zm

Oryginalna [instrukcja](http://zoneminder.readthedocs.io/en/stable/installationguide/ubuntu.html) instalacji.


5. Dodaj kamerę do ZM:
    * Xiaofang:
    ![ZM_Camera1.jpg](http://airmonitor.pl/images/ZM_Camera1.jpg)
    ![ZM_Camera2.jpg](http://airmonitor.pl/images/ZM_Camera2.jpg)
    * Dafang:
    ![xiaomi_dafang_02.jpg](http://airmonitor.pl/images/xiaomi_dafang_02.jpg)
    ![xiaomi_dafang_03.jpg](http://airmonitor.pl/images/xiaomi_dafang_03.jpg)
    ![xiaomi_dafang_04.jpg](http://airmonitor.pl/images/xiaomi_dafang_04.jpg)
    ![xiaomi_dafang_05.jpg](http://airmonitor.pl/images/xiaomi_dafang_05.jpg)

6. Opcjonalnie. Dodaj kamerę do domoticza. Pełny ImageURL to **/zm/cgi-bin/zms?mode=single&monitor=1&** (dla kamery o ID=1):
![Domoticz_camera.jpg](http://airmonitor.pl/images/Domoticz_camera.jpg)

7. Ustawienia redukujące ilość zajmowanego miejsca przez zarejestrowany obraz w ZoneMinder.

    1. Decrease the frame rate.
    2. Decrease the resolution.
    3. Don't create analysed alarm images (Options->Config->CREATE_ANALYSIS_IMAGES)
    4. Decrease the JPEG quality (Options->Images->JPEG_(ALARM)_FILE_QUALITY)
    5. Change alarm zone settings to decrease the number of alarms.



