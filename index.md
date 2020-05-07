# Home Automation

## Inleiding

Ik heb lang gesleuteld en nagedacht over mijn eigen home-automation, en de kennis die ik daarbij het opgedaan wil ik hier stapsgewijs publiceren, in de hoop dat dit anderen helpt met hun eigen automatiseringsprojecten.

## Hardware

### Mosquitto

Hoe maak je de beste en mooiste huisautomatisering? Als eerste door te zorgen dat de communicatie soepel verloopt. En wat hebben we daarvoor nodig? Een goed protocol. En wat is een goed protocol voor huisautomatisering: Mosquitto, oftewel MQTT. Dat is ingrediënt 1.

### Arduino

Waarschijnlijk heb je in je te automatiseren huis al een aantal bedrade sensoren hier en daar, bijvoorbeeld wat raam- en deurmagneten en temperatuursensoren. En je hebt vast ook wat aan te sturen apparaten, zoals c.v.-kleppen, zonneschermen, sirene’s. Kortom je hebt wat bedrade I/O. Aangezien we net gekozen hebben voor MQTT, moeten we iets hebben dat deze sensorinformatie gemakkelijk kan omzetten in MQTT-berichten. Hoe kunnen we dat goedkoop en stabiel en zonder afhankelijk te zijn van één leverancier regelen? Met een Arduino. Arduino’s hebben in- en uitgangen, en ze hebben een netwerkaansluiting, en er bestaan bibliotheken en voorbeeldprogramma’s die doen wat wij willen. Dat is ingrediënt 2.

### Nog een Arduino

We zijn dol op huisautomatisering, maar van sommige onderdelen willen we gewoon dat het altijd werkt: de aansturing van de verwarming bijvoorbeeld. Anders zit je opeens in de kou. Of nog erger, dan zit je vrouw in de kou terwijl je op je werkt bent, en dat is funest voor de WAF. Dus ook al stort je netwerk in, lopen er allerlei componenten vast, dit deel moet bijzonder stabiel zijn en altijd werken. Hoe doen we dat? Met nóg een Arduino: we programmeren de essentiële delen van de regeling in de Arduino zelf, zodat deze onafhankelijk van alle overige apparatuur kan aansturen wat er aangestuurd moet worden. En de benodigde I/O hiervoor sluiten we direct aan op deze Arduino, zodat dit een zelfstandig opererende unit wordt. En we laten deze Arduino ook MQTT praten: hij mag dan via MQTT aan de rest vertellen wat ie doet, en wat de sensorwaarden zijn. En misschien wil je ook wel met MQTT de regeling kunnen beïnvloeden. Kortom, een stabiele zelfstandige regeling voor de kritische zaken: nóg een Arduino, dat is ingrediënt 3.

### Raspberry Pi

Vervolgens wil je iets doen met al die MQTT-berichten, want je wil een plek waar die berichten samenkomen en waar alles geregeld wordt: de aansturing van je verlichting, je verwarming, je beveiliging. En je wil misschien wel een web-interface, of je wil de tijd ophalen van internet, of het weer. Of je wil commando’s via internet kunnen ontvangen. Hoe doen we dat? Met nóg een Arduino? Nee, die zijn daar niet krachtig genoeg voor. Hier is een Raspberry Pi juist weer erg geschikt voor: voldoende geheugen en processorkracht om te dienen als hoofdregeling, en voor de communicatie met de buitenwereld, en voor de afhandeling van web-interfaces. Dat is ingrediënt 4.

### HomeMatic

Verder kun je nog toevoegen wat je wil. Allerlei apparatuur van diverse leveranciers. De enige eis die we stellen, is dat het MQTT leest en schrijft. Alleen met deze eis valt 99% van de apparatuur af, dus noodgedwongen herformuleren we onze eis: het hoeft niet zelf MQTT te lezen te schrijven, maar er moet een interface bestaan (of te maken zijn) waarmee we vervolgens wel via MQTT met die apparaten kunnen communiceren. Ik heb zelf diverse apparaten van het merk [HomeMatic](https://www.eq-3.de/produkte/homematic.html) (radiatorkranen, aan/uit-schakelaars, dimmers, draadloze sensoren). Om verbinding te maken met deze apparaten gebruik je een bridge. Bij HomeMatic heet dat de CCU (central control unit). Ik heb de tweede versie van dit apparaat, de [HomeMatic CCU2](https://www.eq-3.de/produkte/homematic/detail/homematic-zentrale-ccu-2.html). Op dit apparaat kun je extensies/plug-ins/add-ons installeren. Er is een MQTT-interface voor dit apparaat beschikbaar als add-on. En dit is fantastisch, want hiermee spreken al mijn HomeMatic-apparaten nu ook MQTT. Er is alleen één maar: de add-on is beperkt, want het is een oude versie. Er is wel een nieuwere versie, maar die is te zwaar voor de CCU2. Hij draait wel op de CCU3 (de opvolger van de CCU2). Moet ik dan de CCU2 afschrijven en een CCU3 kopen? Nou dat was een optie, maar er is een omweg: de nieuwe add-on draait behalve op de CCU3 ook als add-on op de [Node-RED](https://nodered.org)-omgeving. Maar dan moet ik als extra Node-RED installeren, en ik wilde juist een recht-toe-recht-aan opstelling zonder allerlei tussenliggende lagen. Maar de dreiging om anders de CCU3 te moeten kopen heeft me toch overgehaald Node-RED te installeren op de Raspberry Pi (ingredient 4 dat ik toch al had). En ik moet zeggen: Node-RED ziet er heel goed uit (en is gratis)! Maar zoals gezegd: ik wilde zo min mogelijk met Node-RED te maken hebben dus ik heb een minimale ‘flow’ gemaakt met alleen de paar noodzakelijke componenten om een interface tussen de CCU2 en MQTT te maken. 

## Software

### Python

En dan zijn alle hardware-ingredienten compleet: een stabiele zelfstandige regeling voor de kritische regelingen (verwarming), alle componenten beschikbaar via MQTT, een Raspberry Pi als basis voor de hoofdregeling.
Maar dan? Dan komt het software-deel. Gebruik ik een kant-en-klaar home-automation pakket zoals [openHAB](https://www.openhab.org) of [IP-Symcon](https://www.symcon.de)? Ik heb bij eerdere automatiseringspogingen openHAB gebruikt, en ik liep tegen irritante beperkingen aan op het gebied van integratie met mijn HomeMatic, en het programmeren en debuggen is niet voldoende intuïtief en tijdrovend. Ik wilde gewoon mijn HomeMatic-apparaten kunnen bedienen en niet afhankelijk zijn van wat openHAB nou toevallig wel en niet werkend wil of kan maken. Ik ben daarop overgestapt naar IP-Symcon, geïnspireerd door [Femme](https://www.youtube.com/watch?v=fWcDT4JISn8). Wanneer iemand met veel kennis van home automation voor dit pakket kiest, kan het nooit slecht zijn: beter goed gejat dan slecht bedacht. En IP-Symcon ondersteunt ook de integratie met HomeMatic, dat was ook een eis. Nadeel: IP-Symcon is niet gratis, en hoe meer sensoren je hebt, hoe duurder het wordt. Je kunt gelukkig wel een proefversie draaien met zoveel sensoren als je wil, alleen dan moet je elk uur je homeautomation opnieuw starten. Dat wil je natuurlijk niet, maar als test is het voldoende. Het leuke van de HomeMatic-apparaten is dat ze veel informatie geven: een simpel draadloos deurcontact heeft niet slechts één sensor, maar ook nog sensoren over de batterijtoestand, sabotage, noem maar op. En bij de radiatorkranen kun je het hele weekprogramma meekrijgen als je wil. En elke parameter is een sensor in IP-Symcon. Bij elkaar zijn dat heel erg veel sensoren. En bij dit pakket betekent ‘heel erg veel sensoren’: de duurste versie. En ik kreeg het niet voor elkaar om alleen díe sensoren mee te laten tellen die ik ook echt ging gebruiken, bij IP-Symcon koppel je een HomeMatic-apparaat óf wel óf niet. Alles of niets dus. Ik heb er nog een e-mailtje aan gewaagd aan IP-Symcon of zij daar een oplossing voor hadden. Nou die hadden ze wel: gewoon het duurste pakket kopen! En alhoewel ze daar natuurlijk wel gelijk in hadden, had ik toch geen goed gevoel overgehouden aan de e-mailwisseling, en de gunfactor om geld bij ze uit te geven daalde aanzienlijk. Gelukkig had mijn proefversie geen last van de beloofde eigenschap om elk uur uit zichzelf te stoppen. Bij mij bleef die gewoon draaien, dus ik kon nog rustig nadenken of ik het pakket wel of niet wilde gaan aanschaffen een keer. Op een gegeven moment was ik aan het programmeren in IP-Symcon, om alle gewenste regelingen erin te krijgen, en ik ergerde me aan specifieke kennis die je nodig had van IP-Symcon om alles voor elkaar te krijgen. En het volgde ook niet allemaal mijn intuïtie. En toen bedacht ik me, dat ik eigenlijk helemaal niet afhankelijk wil zijn van software-pakketten met halve integraties, hoge prijzen, ingewikkelde programmeertrucs. Ik wilde gewoon lekker programmeren met iets gebruiksvriendelijks, een programmeertaal die mij begrijpt, en andersom. Lang verhaal kort: tot ziens IP-Symcon en welkom aan software-ingrediënt 1, de programmeertaal Python. Python kan prima overweg met MQTT, en draait prima op de Raspberry Pi. En aangezien alles in de vorm van MQTT aankomt bij de Raspberry Pi, hoeft er alleen nog maar wat geprogrammeerd worden voor de goede aansturing van alles. Vervolgens het stukje Python-programma starten als service, wat logging toevoegen, en alles wordt bestuurd zoals je het zelf wil. Geen urenlange zoektochten meer hoe je bepaalde simpele functies en omrekeningen in jouw specifieke automatieringspakket perst, geen workarounds meer voor de grillen van je pakket, geen moeilijk gezoek in forums, gewoon alles in Python.

### Node-RED

En dan is alles klaar en geregeld:
-	De verwarming wordt zelfstandig aangestuurd met een Arduino, die via MQTT de status meldt en ook bijgestuurd kan worden via MQTT.
-	Alle overige bedrade I/O zit op een tweede Arduino die alleen als taak heeft de I/O uit te lezen en aan te sturen middels MQTT.
-	De Raspberry Pi is verzamelpunt voor de MQTT-berichten en er draait een Python-programma dat het hele huis aanstuurt middels het lezen en sturen van MQTT.
-	Alle draadloze apparatuur wordt door de HM-CCU2 omgezet naar het bedrade netwerk via het eigen protocol van HomeMatic. Dit protocol wordt door een Node-RED add-on omgezet naar MQTT waarna het op dezelfde manier bediend kan worden als de rest.

Job done! Alhoewel, het werkt nu allemaal wel, maar ik zie er niks van. Ik heb geen leuke webpagina met grafiekjes van de C.V.-aansturing, ik zie niet welke ramen en deuren open zijn, ik weet niet of de achterdeur op slot zit. Hoe moeten we dat oplossen? Met een huisautomatiseringspakket dat ik net heb afgeschaft? Mogelijk, en dan die puur gebruiken voor de web-interface, en verder niet voor de regeling. Dat was een optie. Maar omdat ik toch al noodgedwongen Node-RED geïnstalleerd had, ben ik dat eens beter gaan bekijken: het bleek helemaal niet zo’n toeval dat de add-on-maker overgestapt was naar Node-RED. wie Node-RED zegt, zeg ook MQTT. En doordat ik heel snel die koppeling tussen MQTT en HomeMatic had geregeld in Node-RED, had er ook wel een belletje bij me moeten gaan rinkelen: Node-RED is een prima toevoeging voor mijn home-automation omdat het intuïtief werkt en stabiel is, en gewoon goed werkt! En het heeft een web-interface, met knopjes en schakelaars. Dus toen heb ik me maar gestort op Node-RED om het ui-deel van mijn homeautomation erin te zetten, oftewel: software-ingrediënt 2: Node-RED

### Daikin airconditioner

Sinds kort hebben we een airconditioner. Als ik groen wil overkomen, noem ik het een lucht-lucht-warmtepomp. Hij kan ook verwarmen namelijk. En dat doet ie heel efficiënt, dus misschien ben ik wel echt groen, alhoewel geen airco vast groener is. Hij is van het merk Daikin en er zit een wifi-module in. Je kunt met een app op je telefoon de airco bedienen. Maar je kunt hem ook bedienen via HTML. En wat zou er nu leuker zijn dan een interface tussen mijn op MQTT-gebaseerde automatisering en de op HTML gebaseerde wifi-module van mijn Daikin? Ik heb zo'n interface gemaakt in Python, en die als service laten draaien op mijn Raspberry Pi. Mocht je zelf ook MQTT gebruiken, en je hebt een Pi, en je hebt een Daikin airco met wifi-module, dan is deze interface misschien ook wat voor jou: het is alleen een stukje software en ik heb het [hier](https://github.com/floeplala/daikin-mqtt) gepubliceerd. Heb je tips, verbeteringen, aanvullingen of problemen? Maak dan daar een issue aan.

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
