---
layout: default
title: Communicatie
parent: De vuilbak
grand_parent: Puzzels
nav_order: 5
---
# Communicatie
Doorheen de escaperoom zijn er heel wat zaken waarmee de vuilbak moet **communiceren**.
Zo moet de vuilbak doorheen het gehele spel op de hoogte worden gehouden wanneer de buffer in de groene zone komt en wanneer de buffer weer uit de groene zone gaat.
(De vuilbak mag enkel actief zijn wanneer de buffer zich in de groene zonde bevindt.)
Daarnaast moet er ook een code ontvangen worden waarvan de eerste cijfers verstuurd worden vanaf "'k ga meegaan" en het laatste cijfer verstuurd wordt vanaf "De trein is altijd een beetje reizen".
Alsook moet de vuilbak meldingen geven van fouten die de spelers maken tijdens het oplossen van de puzzel aan de buffer.
Ten slotte wordt aan het einde van de puzzel een code doorgestuurd naar het UV-licht.

*Voor een grafisch overzicht over wanneer er precies gecommuniceerd wordt doorheen de loop van het spel zie de [deze](https://plan-it-b.github.io/ba3-docs/docs/DeVuilbak/DeVuilbak.html) pagina voor de flowchart.*

## Publishen
Wanneer we iets willen versturen naar de MQTT server kunnen we dit doen aan de hand van het **client.publish(X,Y)** commando.
Hierin is X de directory waarin we het bericht willen plaatsen en Y is het te versturen bericht zelf.
De andere componenten van het systeem kunnen wat de vuilbak gestuurd heeft ontvangen door te subscriben op de correcte directory.
Het bericht wordt dus niet rechtstreeks gestuurd naar andere puzzels maar naar de gemeenschappelijke broker.
De MQTT-server zet een message die hij ontvangt op een bepaalde directory en deze wordt doorgestuurd naar alle partijen die gesubscribed zijn op die directory.

In de volgende listing ziet u de verschillende directories waar wij naar publishen met daaronder het exacte bericht dat gepublished wordt naar die specifieke directories. 

- controlpanel/status
  - "Garbage Ready"
  - "Garbage code is correct ingegeven"
- TrappenMaar/buffer
  - "grote fout"
- Wristbands
  - "Stop Wristbands"
- garbage/eindcode
  - een int van vier cijfers.

## Subscriben

Behalve zaken publishen moet de vuilbak natuurlijk ook van bepaalde zaken op de hoogte gebracht worden.
Dit gebeurt zoals hierboven kort beschreven door te subscriben op een bepaalde directory.
*Voor meer informatie over de werking van een MQTT-server klik [hier](https://plan-it-b.github.io/ba3-docs/Communicatie.html).*

In de volgende listing worden de directories beschreven met daarbij vermeld welke berichten we van deze directories verwachten

- wristbands/# : Vanuit deze directory verwachten we een bericht met daarin de eerste cijfers van de code voor het openen van de vuilbak
- treingame/# : Vanuit deze directory verwachten we een bericht met daarin het laatste cijfer van de code voor het openen van de vuilbak
- controlpanel/reset : Vanuit deze directory verwachten we een bericht wanneer de escape room gereset dient te worden. 
- TrappenMaar/zone : vanuit deze directory verwachten we een bericht met daarin de zone waarin de buffer zich bevindt. (Dit vindt enkel plaats bij een wijziging van de zone.)
- garbage/eindcode : In deze directorie publishen we de eindcode van de vuilbak.
Er wordt enkel gesubscribed op deze directory ter controle.

De # betekend dat het programma zal luisteren naar boodschappen in elke directory onder bijvoorbeeld treingame.







## Diagram
![](Communicatie_Diagram.png)

