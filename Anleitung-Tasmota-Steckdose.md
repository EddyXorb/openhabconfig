# Tasmota Steckdosen einrichten
## Firmware flashen
Um Steckdosen von *gosund* mit der Tasmotafirmware zu bespielen, benutzt man tuya-convert von c't.
Unbedingt nach dem Flashen das richtige WLAN eingeben, ansonsten kann man durch sechsmaliges rein und raus ziehen der Steckdose die WLAN-Einstellung zurücksetzen.

Das Template für Steckdosen lautet:
```
{"NAME":"Gosund SP1 v23","GPIO":[0,576,0,32,2720,2656,0,0,2624,320,224,0,0,0],"FLAG":0,"BASE":55}
```
Dieses muss in Configuration -> Other ganz oben eingegeben werden.

Danach noch unter Mqtt -> user/password und host die Daten für den bereits aufgesetzten mqtt-Server eingeben. Am besten *mosquitto* nehmen, kann man auch im Docker laufen lassen.
## Steckdosen kalibrieren
1. Steckdose mit Glühlampe verbinden (Halogen oder Glühfaden)
2. Mit Tasmota-Webseite verbinden
3. Console öffnen
4. Nacheinander die Werte für Watt, Spannung und Stromstärke eingeben. Hier ein Beispiel für eine 40-Watt Lampe:
   1. Powerset 40 + ENTER
   2. Voltageset 230 + ENTER
   3. Currentset 173.9 + ENTER