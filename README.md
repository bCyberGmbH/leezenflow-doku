# Readme.md

  

# Leezenflow Dokumentation

  

## Einleitung

  

![](https://user-images.githubusercontent.com/66736282/129550691-5f99209a-2266-44ea-8525-ba5e195e8848.png)

Foto: Stadt Münster, Meike Reiners

  

[Leezenflow ist ein Grüne-Welle-Assistent](https://smartcity.ms/leezenflow/) für Fahrräder. Er visualisiert die Restzeit der aktuellen Ampelphase und unterstützt dabei das richtige Tempo zu finden, um bei Grün über die nächste Ampel zu fahren.

  

Dieses Dokument dient primär der technischen Dokumentation.

  

## Andere Repositories

  

*   [Leezenflow Code](https://github.com/bCyberGmbH/leezenflow-code)
*   [Leezenflow Design](https://github.com/bCyberGmbH/leezenflow-design)


## Leezenflow V2

Diese Dokumentation beschreibt die neuste Version des Leezenflow.
Die folgenden Dinge habe sich an der Hardware geändert:

- geänderte Stromversorgung auf 12V Eingangsspannung (vorher 230V)
- Einbau einer OBU in das Gehäuse (vorher Datenempfang über extern montierte RSU)
- Sensoren für Temperatur, Luftfeuchtigkeit, Luftdruck und Eingangsspannung
- LTE Router für Fernzugriff und Monitoring

Eine Schematische Darstellung der verbauten Hardware ist auf [diesem Bild](/bilder/leezenflow-aufbau.svg) zu erkennen.

## Kollaboration

  

Wir freuen uns über konstruktive Mitarbeit 👍

  

| Beschreibung | Tool |
| ---| --- |
| Diskussionen und Fragen | [GitHub Discussions](https://github.com/bCyberGmbH/leezenflow-doku/discussions) |
| ToDo's + geplante Änderungen | [GitHub Issues](https://github.com/bCyberGmbH/leezenflow-doku/issues) |
| Änderungen an dieser Doku | [GitHub Pull-Requests](https://github.com/bCyberGmbH/leezenflow-doku/pulls) |

  

Im Zweifel ist der richtige Einstiegspunkt immer eine Diskussion in den [GitHub Discussions](https://github.com/bCyberGmbH/leezenflow-doku/discussions) :wink:

  

## Komponenten

  

**Leezenflow**

  

Die wohl offensichtlichste Komponente zur Anzeige des Ampelstatus.

Eine genaue Beschreibung der Komponenten und die Dokumentation zum nachbauen befindet sich [hier](http://case.md).

  

**RSU (Road Side Unit)**

  

Für den Prototyp haben wir mit einem Gerät von Swarco gearbeitet. Die RSU wird in der Regel für die "Vehicle-to-everything" Kommunikation eingesetzt. Zum Beispiel kann damit die Ampelphase an (dafür ausgerüstete) Autos übertragen werden. Für das Projekt Leezenflow verwenden wir ein RSU Gerät, dass als Sender per Kabelverbindung mit dem Ampelsteuergerät verbunden ist. Die RSU kann mit einem LAN-Kabel betrieben werden, da diese per Power-over-Ethernet (PoE) mit Strom versorgt wird.

  

**OBU (On-Board Unit)**

  

Zum Empfang der Daten verwenden wir eine OBU. Diese empfängt die Daten, die von der Ampel (RSU) ausgesendet werden und leitet diese an unser Leezenflow Gerät weiter.

  

**Ampelsteuergerät**

  

Dem Steuergerät der Ampel wurde ein spezielles Prognose-Modul (Software) hinzugefügt, welches per [MQTT](https://de.wikipedia.org/wiki/MQTT) die Ampelphasen an die Sender RSU übermittelt. Diese Software muss vom Ampelhersteller implementiert werden.

  

**Fahrrad-Ampel**

  

An der Ampel selbst ist keinerlei Änderung nötig. Diese wird weiterhin vom Ampelsteuergerät geregelt. Leezenflow hat keinen Rückkanal und dient nur der Visualisierung der Schaltzeiten dieser Ampel.

  

## Detaillierte Anleitungen

  

*   [Leezenflow-Gehäuse bauen](case.md)
*   [Leezenflow Field-Guide](field-guide.md)

  

## Datenfluss

  

```plain
🚲 <-- 📡(OBU) <** 📡(RSU) <-- 💻 --> 🚦
```

  

Legende:

  

*   🚲 = Leezenflow Anzeige
*   📡(RSU) = Road Side Unit
*   📡(OBU) = On-Board Unit
*   💻 = Ampel Steuergerät
*   🚦 = Fahrrad Ampel
*   \*\* = Funkverbindung
*   \-- = (LAN)Kabelverbindung

  

## Passwörter / sicherheitsrelevante Information

  

Ansprechpartner für Zugangsdaten und Gerätekonfigurationen ist [bCyber](https://www.bcyber.de).

  

## Projektpartner

  

### [Stabsstelle Smart City, Stadt Münster](https://smartcity.ms)

  

*   Projektkoordination

  

### [Amt für Mobilität und Tiefbau, Stadt Münster](https://www.stadt-muenster.de/tiefbauamt)

  

*   Auftraggeber
*   Umsetzung von nötigen Baumaßnahmen

  

### [bCyber](https://www.bcyber.de)

  

*   Technische Umsetzung und Programmierung

  

### [Code for Münster](https://codeformuenster.org/)

  

*   Unterstützung bei Umsetzung und Programmierung

  

### [FH Münster, Münster School of Design](https://www.fh-muenster.de/msd/index.php)

  

*   Design
*   Bachelorarbeit von [Magdalena Schmitz](https://wise20.parcours-muenster.de/arbeiten/magdalena-schmitz/) und [Leonie Winkelmann](https://wise20.parcours-muenster.de/arbeiten/leonie-winkelmann/)

  

### [Swarco](https://www.swarco.com/de)

  

*   Technische Umsetzung der Übertragung von Ampeldaten

  

### [Universität Münster, Institut für Verkehrswissenschaft](https://www.wiwi.uni-muenster.de/ivm/)

  

*   [Evaluation](https://www.stadt-muenster.de/sessionnet/sessionnetbi/vo0050.php?__kvonr=2004049785)

  

## Unterstützer

  

### [MÜNSTERHACK](https://muensterhack.de/)

  

### [DB GoBeta](https://gobeta.de)

  

### [Zweitag](https://www.zweitag.de/)
