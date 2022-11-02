# Installation docker-container
1. Vorliegendes `docker-compose-example.yml` zu `docker-compose.yml` umbennen und in eigenen Ordner `openhab/` packen, wo später auch die gesamte Konfiguration abgelegt werden soll.
2. Mit SSH in den Ordner gehen und `sudo docker-compose up -d` ausführen -> das installiert openhab und gleich einen zuverlässigen *mqtt*-broker.
# Bedienung Openhab
3. `localhost:8899` öffnen im Browser (bzw. die IP des servers) -> da sollte nun openhab erscheinen und man kann das Adminpasswort vergeben
4. Im Ordner `openhab/config` befindet sich die gesamte Konfiguration des Openhab Servers, einschließlich der *Things* und *Items* und *Channels* der *Things*. Beispiele wie man diese befüllt sind in diesem Repository und [hier](https://openhabdoc.readthedocs.io/de/latest/Beispiel/) zu finden.
5. Weitere wichtige Dokumentationen zu Openhab finden auf der offiziellen Webseite, leider nur in Englisch. Die Kernidee besteht meiner Meinung nach im sogenannten *semantischen Modell*, das [hier](https://www.openhab.org/docs/tutorial/model.html) erklärt wird.
6. Es gibt zwei Arten *Things*,*Items* etc zu OpenHAB hinzuzufügen: per WEB-UI und per Textfile. Textfile ist auf Dauer deutlich übersichtlicher. Man kann, sollte aber nicht beides mischen, da das Textfile eine Einbahnstraße ist und nicht Web-UI-Änderungen darin reflektiert werden und man dann nicht mehr weiß, was die alleinige Quelle der Wahrheit ist (bzw. gibt es dann zwei - schlecht).
7. Tipp: Um die Config von Openhab leichter zu manipulieren, ein Netzlaufwerk, das auf die config zeigt einrichten
8. Tipp: In *Visual Code* gibt es ein Addon, das die Syntax von OpenHAB beherrscht, Färbungen und Formatierungen bietet. Auérdem kann man sich mit dem Addon mit dem OpenHAB-Server verbinden und man bekommt alle *Things* etc angezeigt.
9. Tipp: Immer Schritt für Schritt neue Dinge zur Config hinzufügen und checken, ob es klappt. Die Webseite aktualisiert man mit F5 am besten gleich nach einer Änderung. Man sieht dann sofort, ob es den gewünschten Effekt hatte oder nicht - oft hat man die Syntax falsch, dann geht was nicht, man bekommt aber leider auch keine Fehlermeldung darüber (habe ich zumindest noch nicht gefunden wo).
# Einrichtung MQTT-Broker
1. Im Ordner `openahb/mqtt_config` sollte so etwas eingetragen werden:
```
listener 1883

persistence true

persistence_location /mosquitto/data/

log_dest file /mosquitto/log/mosquitto.log

allow_anonymous false
password_file /mosquitto/data/pwfile
```
2. Als nächtes muss man in den docker container von mqtt reingehen und dort von darinnen einen Benutzer und ein passwort anlegen, mit dem Befehl hier:
`mosquitto_passwd -b /mosquitto/data/pwfile mymqttuser mymqttpassword`
Damit ist sichergestellt, dass der Zugriff abgesichert ist, und durch den vorherigen Punkt wird der Benutzer persistent, selbst wenn der dockercontainer geupdated wird.
3. In der Openhab-config unter `things` eine Datei `broker.things` wie in dem Beispiel anlegen und die Zeile
```Bridge mqtt:broker:mymqtt [ host="mqtt",secure=false, username="mymqttuser", password="mymqttpassword" ]```
einfügen ganz oben - damit hat openhab nun Zugriff auf den Broker. Erkärung: *host=mqtt* gibt OpenHAB die Adresse, wo mosquitto (der MQTT-Broker) zu finden ist. Da in dem docker-compose-file *mosquitto* mit dem Namen **mqtt** benannt wurde, und docker in dem virtuellen Netz die Name der container auf die IP-Adressen des virtuellen Netzen per DNS umlegt, findet OpenHAB das ohne Probleme.