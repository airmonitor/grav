---
title: Schemat
taxonomy:
    category: docs
---
###Wersja podstawowa w oparciu o sensor SDS* oraz Raspberry Pi 0 W

Dla sensorów Nova SDS* wykorzystamy zasilanie z USB oraz piny UART.

>>>>> Zasilanie z USB eliminuje problem restartów raspberry pi gdy zasilanie dla sensora SDS jest dostarczane bezpośrednio z pinów. Zasilamy z USB jeszcze z jednego powodu, sensory SDS mają relatywnie krótką żywotność a jest to 8000h nieprzerwanej pracy. Stąd też sensor po cyklu pracy będzie usypiany i wybudzany przed kolejnym cyklem pomiarowym.

![Schemat wyprowadzenia przewodów dla SDS021/11/18](http://airmonitor.pl/images/image20.jpg)

**Przewód niebieski od sensora łączymy z pinem 10, natomiast przewód żółty z pinem 8.** 

Tak wyglądałaby wersja podstawowa, możnaby rzec biurkowa urządzenia, które mierzy pyły PM2,5 oraz PM10.


###Wersja rozszerzona w oparciu o sensor SDS* oraz Raspberry Pi 0 W

Umieśćmy sensor pyłów w puszce. Dzięki temu zabiegowi będziesz w stanie dokonywać pomiarów na świeżym powietrzu bądź bardziej zamaskować urządzenie w domu - chowając je pomiędzy książkami ;)

>>>Puszka ma to do siebie, że ogranicza przepływ powietrz. Aby zapewnić jego optymalny ruch konieczne jest zastosowanie wentylatora.

1. Podłącz wentylator do raspberry pi.
![Schemat wyprowadzenia przewodów dla wentylatora](http://airmonitor.pl/images/image1.jpg)

2. Używając pilnika powiększ otwory w puszce.
![Obudowa](http://airmonitor.pl/images/image4.jpg)

3. Przyklej [wydruk 3D](https://github.com/airmonitor/home_air_monitor/blob/master/80x80x40mm.stl) w powiększony w poprzednim kroku otwór. Jeśli nie posiadasz drukarki 3D napisz do Nas wiadomość, prześlemy Tobie wydruki 3D.
![Przyklejona do obudowy osłona na deszcz.](http://airmonitor.pl/images/image12.jpg)

4. Powiększ kolejny otwór znajdujący się na środku puszki oraz przyklej wydruk 3D.
![Przyklejona do obudowy druga osłona na deszcz](http://airmonitor.pl/images/image17.jpg)

5. Wentylator o rozmiarze 40 x 40 mm przymocuj za pomocą kleju na gorąco jak na poniższym zdjęciu. 
>>>>>Pamiętaj, aby umieścić wentylator w ten sposób, aby powietrze wydmuchiwane było skierowane na zewnątrz obudowy.

![Przyklejony do obudowy wentylator.](http://airmonitor.pl/images/image23.jpg)

>>>>>Wentylator zasilany jest napięciem 3V, a nie 5V. Ma to na celu wydłużenie czasu pracy wentylatora, a ponadto nie jest potrzebny, aż taki ruch powietrza, jak w przypadku, gdy wentylator jest zasilany napięciem 5V.

![Przyklejony do obudowy wentylator (w zbliżeniu).](http://airmonitor.pl/images/image10.jpg)

6. Umieść komponenty w obudowie wykorzystując poniższą fotografię.
![Cały zestaw mierzący PM10 oraz PM2,5.](http://airmonitor.pl/images/image19.jpg)


###Wersja podstawowa w oparciu o [PMS7003](http://allegro.pl/show_item.php?item=7097428244) oraz Raspberry Pi 0 W

Dla sensora Plantower [PMS7003](http://allegro.pl/show_item.php?item=7097428244) wykorzystamy wyłącznie piny celem zasilania jak i komunikacji.

>>>Sensor Plantower [PMS7003](http://allegro.pl/show_item.php?item=7097428244) będzie pracował bez hibernacji, z tego powodu możemy wykorzystać wyłącznie piny Raspberry Pi.

![Schemat połączenia Raspberry Pi 0 W z [PMS7003](http://allegro.pl/show_item.php?item=7097428244).](http://airmonitor.pl/images/image26.jpg)

>>>>>Pamiętaj port TXD z RPi0W połącz z RXD na [PMS7003](http://allegro.pl/show_item.php?item=7097428244) oraz RXD z RPi0W połącz z TXD na [PMS7003](http://allegro.pl/show_item.php?item=7097428244) 

###Wersja w oparciu sensor SDS* oraz EspEasy Mega na przykładzie Wemos D1 mini v2
####Schemat połączenia
![Schemat połączenia dla SDS021/11/18](http://airmonitor.pl/images/espeasy_sds_schema.jpg)

####Konfiguracja EspEasy Mega
![konfiguracja_espeasy_mega_bme280](http://airmonitor.pl/images/espeasy_sds_configuration.jpg)

3. W zakładce **Devices** dodaj sensor Nova SDS011/018/198/021 postępując według poniżej ilustracji:
![Zrzut ekranu prezentujący zakładkę devices](http://airmonitor.pl/images/image29.jpg)



