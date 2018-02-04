---
title: Schemat
taxonomy:
    category: docs
---
###Wersja podstawowa w oparciu o SDS*

Dla sensorów Nova SDS* wykorzystamy zasilanie z USB oraz piny UART.

>>>>> Zasilanie z USB eliminuje problem restartów raspberry pi gdy zasilanie dla sensora SDS jest dostarczane bezpośrednio z pinów. Zasilamy z USB jeszcze z jednego powodu, sensory SDS mają relatywnie krótką żywotność a jest to 8000h nieprzerwanej pracy. Stąd też sensor po cyklu pracy będzie usypiany i wybudzany przed kolejnym cyklem pomiarowym.

![Schemat wyprowadzenia przewodów dla SDS021/11/18](http://airmonitor.pl/images/image20.jpg)

**Przewód niebieski od sensora łączymy z pinem 10, natomiast przewód żółty z pinem 8.** 


Tak wyglądałaby wersja podstawowa, możnaby rzec biurkowa urządzenia, które mierzy pyły PM2,5 oraz PM10.


###Wersja deluxe

Umieśćmy sensor pyłów w puszce. Dzięki temu zabiegowi będziesz w stanie dokonywać pomiarów na świeżym powietrzu bądź bardziej zamaskować urządzenie w domu - chowając je pomiędzy książkami ;)

>>>Puszka ma to do siebie, że ogranicza przepływ powietrz. Aby zapewnić jego optymalny ruch konieczne jest zastosowanie wentylatora.

1. Podłącz wentylator do raspberry pi.
![Schemat wyprowadzenia przewodów dla wentylatora](http://airmonitor.pl/images/image1.jpg)

2. Używając pilnika powiększ otwory w puszce.
![Obudowa](http://airmonitor.pl/images/image4.jpg)

3. Przyklej wydruk 3D w powiększony w poprzednim kroku otwór.
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


###Wersja podstawowa w oparciu o PMS7003

Dla sensora Plantower PMS7003 wykorzystamy wyłącznie piny celem zasilania jak i komunikacji.

>>>>Sensor Plantower PMS7003 będzie pracował bez hibernacji, z tego powodu możemy wykorzystać wyłącznie piny Raspberry Pi.

###Wersja podstawowa w oparciu o EspEasy Mega

Opis dotyczy wersji mega-20180126.
Układ komponentów w puszce identyczny.

