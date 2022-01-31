# Field Guide

In diesem Dokument finden sich hilfreiche Tipps für den Umgang mit Problemen im Live-Betrieb.

## WiFi Zugriff am Gerät

Leezenflow verbindet sich zu einem vordefiniertem Wartungs-Netzwerk. Dieses kann man z.B. per Handy Hotspot oder geteilter Verbindung vom Laptop erstellen.
Nutzt man die richtigen Zugangsdaten (SSID und Passwort) sollte sich Leezenflow nach einer kurzen Zeit automatisch verbinden.
Es ist der bCyber SSH "Wartungskey" hinterlegt. Man kann also z.B. mit
`ssh -i .ssh/bcyber pi@LEEZENFLOWIP` eine Verbindung herstellen.
Ansprechpartner für Zugangsdaten ist [bCyber](https://www.bcyber.de).

## Leezenflow Service

Der Python Code läuft als system.d Service und kann mit den folgenden Befehlen gesteuert werden:

Leezenflow stoppen: `sudo service leezenflow stop`  
Leezenflow starten: `sudo service leezenflow start`  
Leezenflow neustarten: `sudo service leezenflow restart`  
Leezenflow Status: `sudo service leezenflow status`  

Außerdem kann die Ausgabe vom Log mit: `journalctl -u leezenflow` angezeigt werden. Mit `journalctl -u leezenflow -f` bleibt die Ausgabe offen und streamt alle neuen Logs.

## Leezenflow Zeitplan

Mit dem Programm `at` kann man Leezenflow Zeitgesteuert an und ausschalten. Das Vorgehen dabei ist wie folgt:

Als Root User (`sudo su`)  
`at -M 12:00 30.06.2021` eingeben. Der Flag `-M` verhindert, dass bei der Ausführung versucht wird eine E-Mail zu senden! Also benutzen!

Dann bekommt man eine Eingabeaufforderung, erkennbar an dem `at>` im Promt. Hier kann nun der Befehl, z.B. `service leezenflow stop`, eingegeben werden, diesen mit `Enter` bestätigen und dann mit `Strg` + `d` den Prompt beenden. Danach erscheint eine Bestätigung wie `job JOBNUMMER JOBDATUM` als Bestätigung, dass der Befehl geklappt hat.

Mit `atq` kann man sich eingerichtete Jobs anschauen.  

Mit `at -c JOBNUMMER` kann man sich den genauen Inhalt des Jobs anschauen. Der Befehl ist in einem Script gewrappt. Also nicht von der langen Ausgabe beirren lassen 😉  

Mit `at -d JOBNUMMER` kann man einen Job löschen.  

Weitere Informationen findet man auch in der `at` Dokumentation: https://linux.die.net/man/1/at  

## Netzwerk Zugriff im Steuergerät-Kasten

Das Netzwerk im Steuergerät hat eine direkte Verbindung in das städtische Netzwerk. Man könnte somit z.B. aus Versehen auf andere Geräte zugreifen. Eine einfache Möglichkeit das zu verhindern ist es, den Laptop nicht in den vorhanden Switch einzustecken, sondern das vorhandene Kabel zwischen Switch und PoE-Injector abzustecken und mit einem RJ45 zu RJ45 Adapter zu arbeiten. So, dass man physikalisch nur eine Verbindung zum PoE-Injector und der Sender-RSU hat.

## Kein Empfang

Ein Problem (z.B. nach einem Stromausfall) kann sein, dass die `rsu.conf`-Datei auf dem Empfänger "zurückgesetzt" wurde.

Dazu empfiehlt sich folgendes Vorgehen:

```
# ip adresse an die IP von Leezenflow anpassen, ist abhängig vom eigenen Hotspot
ssh -L 8000:127.0.0.1:8000 pi@192.168.2.2
# vom PI aus Verbindung zur RSU herstellen und den Port fürs Webinterface weiterleiten
ssh -L 8000:127.0.0.1:443 user@10.12.6.112
# prüfen ob rsu.conf eine Größe von 0 hat
ls -lah /opt/knopflerfish/osgi/lib/
# Richtige rsu.conf kopieren
cp /home/user/rsu.conf /opt/knopflerfish/osgi/lib/
# zum Aktivieren der neuen Konfiguration einmal die RSU neustarten mit:
sudo reboot now
# dann wieder verbinden
ssh -L 8000:127.0.0.1:443 user@10.12.6.112
```

nun im Browser https://127.0.0.1:8000 aufrufen. Wichtig: Man muss https nutzen und dem Browser erlauben, das selbstsignierte Zertifikat zu nutzen. Brave/Chrome machen dort manchmal Probleme. Mit Firefox kann man in der "Fehleranzeige" das Zertifikat aktzeptieren.

Status vom swarco Service anzeigen:
`systemctl status swarco`

Logs vom Swarco Service anzeigen:
`journalctl -u swarco -f`
