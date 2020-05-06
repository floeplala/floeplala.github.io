## Home Automation

### Inleiding

Ik heb lang gesleuteld en nagedacht over mijn eigen home-automation, en de kennis die ik daarbij het opgedaan wil ik hier publiceren, in de hoop dat het anderen helpt met hun automatiseringsprojecten.

### Mosquitto

Hoe maak je de beste en mooiste huisautomatisering? Als eerste door te zorgen dat de communicatie soepel verloopt. En wat hebben we daarvoor nodig? Een goed protocol. En wat is een goed protocol voor huisautomatisering: Mosquitto, oftewel MQTT. Dat is ingrediënt 1.

### Arduino

Waarschijnlijk heb je in je te automatiseren huis al een aantal bedrade sensoren hier en daar, bijvoorbeeld wat raam- en deurmagneten en temperatuursensoren. En je hebt vast ook wat aan te sturen apparaten, zoals c.v.-kleppen, zonneschermen, sirene’s. Kortom je hebt wat bedrade I/O. Aangezien we net gekozen hebben voor MQTT, moeten we iets hebben dat deze sensorinformatie gemakkelijk kan omzetten in MQTT-berichten. Hoe kunnen we dat goedkoop en stabiel en zonder afhankelijk te zijn van één leverancier regelen? Met een Arduino. Arduino’s hebben in- en uitgangen, en ze hebben een netwerkaansluiting, en er bestaan bibliotheken en voorbeeldprogramma’s die doen wat wij willen. Dat is ingrediënt 2.

### Nog een Arduino

We zijn dol op huisautomatisering, maar van sommige onderdelen willen we gewoon dat het altijd werkt: de aansturing van de verwarming bijvoorbeeld. Anders zit je opeens in de kou. Of nog erger, dan zit je vrouw in de kou terwijl je op je werkt bent, en dat is funest voor de wa-factor. Dus ook al stort je netwerk in, lopen er allerlei componenten vast, dit deel moet bijzonder stabiel zijn en altijd werken. Hoe doen we dat? Met nog een Arduino: we programmeren de essentiële delen van de regeling in de Arduino zelf, zodat deze onafhankelijk van alle overige apparatuur kan aansturen wat er aangestuurd moet worden. En de benodigde I/O hiervoor sluiten we direct aan op deze Arduino, zodat dit een zelfstandig opererende unit wordt. En we laten deze Arduino ook MQTT praten: hij mag dan via MQTT aan de rest vertellen wat ie doet, en wat de sensorwaarden zijn. En misschien wil je ook wel met MQTT de regeling kunnen beïnvloeden. Kortom, een stabiele zelfstandige regeling voor de kritische zaken: nog een Arduino, dat is ingrediënt 3.

### Raspberry Pi

Vervolgens wil je iets doen met al die MQTT-berichten, want je wil een plek waar die berichten samenkomen en waar alles geregeld wordt: de aasnturing van je verlichting, je verwarming, je beveiliging. En je wil misschien wel een web-interface, of je wil de tijd ophalen van internet, of het weer. Of die commando’s via internet kan ontvangen. Hoe doen we dat? Met nóg een Arduino? Nee, die zijn daar niet krachtig genoeg voor. Hier is een Raspberry Pi juist weer erg geschikt voor: voldoende geheugen en processorkracht om te dienen als hoofdregeling, en voor de communicatie met de buitenwereld, en voor de afhandeling van web-interfaces. Dat is ingrediënt 4.

### HomeMatic

Verder kun je nog toevoegen wat je wil. Allerlei apparatuur van diverse leveranciers. De enige eis die we stellen, is dat het MQTT leest en schrijft. Alleen met deze eis valt 99% van de apparatuur af, dus noodgedwongen herformuleren we onze eis: het hoeft niet zelf MQTT te lezen te schrijven, maar er moet een interface bestaan (of te maken zijn) waarmee we vervolgens wel via MQTT met die apparaten kunnen communiceren. Ik heb zelf diverse apparaten van het merk HomeMatic (radiatorkranen, aan/uit-schakelaars, dimmers, draadloze sensoren). Om verbinding te maken met deze apparaten gebruik je een bridge. Bij HomeMatic heet dat de CCU (central control unit). Ik heb de tweede versie van dit apparaat, de HomeMatic CCU2. Op dit apparaat kun je extensies/plug-ins/add-ons installeren. Er is een MQTT-interface voor dit apparaat beschikbaar als add-on. En dit is fantastisch, want hiermee spreken al mijn HomeMatic-apparaten nu ook MQTT. Er is alleen één maar: de add-on is beperkt, want het is een oude versie. Er is wel een nieuwere versie, maar die is te zwaar voor de CCU2. Hij draait wel op de CCU3 (de opvolger van de CCU2). Moet ik dan de CCU2 afschrijven en een CCU3 kopen? Nou dat was een optie, maar er is een omweg: de nieuwe add-on draait behalve op de CCU3 ook als add-on op de Node-RED-omgeving. Maar dan moet ik als extra Node-RED installeren, en ik wilde juist een recht-toe-recht-aan opstelling zonder allerlei tussenliggende lagen. Maar de dreiging om anders de CCU3 te moeten kopen heeft me toch overgehaald Node-RED te installeren op de Raspberry Pi (ingredient 4 dat ik toch al had). En ik moet zeggen: Node-RED ziet er heel goed uit (en is gratis)! Maar zoals gezegd: ik wilde zo min mogelijk met Node-RED te maken hebben dus ik heb een minimale ‘flow’ gemaakt met alleen de paar noodzakelijke componenten om een interface tussen de CCU2 en MQTT te maken. 

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text
[Link](url) and ![Image](src)

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Manuel83/sample/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
