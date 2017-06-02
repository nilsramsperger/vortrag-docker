# Docker
## Eine Einführung

---

## Agenda
* Was ist Docker
* Wie funktioniert Docker
* Lokale Nutzung
* Images erstellen mit Dockerfiles
* Docker Infrastruktur
* Nutzung von Docker Swarm
* Stolpersteine
* Sicherheit

---

## Was ist Docker
### Leichtgewichtige Conatiner Virtualisierung

+++

### Das Problem

* Mehrere Dienste auf einem System
* Geiteilte Laufzeitumgebung
* Abhängigkeiten werden ggf. in verschiedenen Versionen benötigt
* Dienste beeinflussen sich gegenseitig
* Schwer zu überblickende Situation

+++

### Die (mögliche) Lösung

* Virtuelle Machinen
* Hoher Verbrauch von Resourcen
* Hoher Overhead
* Träges Starten und Stoppen von VMs

+++

### Die (bessere) Lösung

* Container Virtualisierung mit **Docker**
* Isolation durch Kernel Features
  * Beschränkung des Zugriffs auf Resourcen _(cGroups)_
  * Sandboxing _(Kernel Namespaces)_
  * Eigene Netzwerk-Stacks je Container
* Verringerter Overhead durch Wegfall des Gastsystems
  * Verringerter Resourcenverbrauch
  * Schnelles Starten und Stoppen von Containern

---

## Wie funktioniert Docker
### Die Bestandteile und ihre Funktion

+++

### Docker Engine

* Läuft als Daemon auf dem Host-System
* Verfügbar für Linux, Windows und OSX
* Stellt Socket zur Ansteuerung zur Verfügung
* Ansteuerung über Konsole oder andere Oberflächen
  * Kitematic
  * DockerCloud

+++

### Images

* Dateisystemabbild (Statische Daten)
  * Laufzeitumgebung
  * Programme
* Werden auf BaseImages aufgesetzt
  * Gängige Linux Distributionen
  * Ubuntu, Debian und Alpine sind populär

+++

### Container

* Veränderbare Kopie eines Image
* Konfiguration
  * Volumes
  * Netzwerk
* Zustandsbehaftet

+++

### Volumes

* Persistente Speicherung von Daten
  * Konfiguration bei Erstellung des Containers
* Werden in Container Dateisystem eingehängt
* Bleiben erhalten, wenn Container gelöscht wird
* Dateiaustausch zwischen Containern

+++

### Named Volumes

* Volume wird über Namen identifiziert
* Wird im Dateisystem des Hosts abgelegt
* Verteilte Speicherung möglich

+++

### Host Volumes

* Es wird direkt ein Pfad auf dem Host verwendet
* Einfacher Datenaustausch zwischen Host und Container möglich

+++

### Netzwerk

* Docker Engine erstellt virtuelles Netz auf Host
* Jeder Container hat eigene IPv4 Adresse
* Erreichbarkeit von außen über automatische NAT
  * Konfiguration bei Erstellung des Containers
* IPv6 ist möglich, untergräbt aber das Prinzip der Transparenz
  * Container sollten nach außen nicht sichtbar sein.

---

## Lokale Nutzung
### Befehle zur Steuerung von Docker

+++

### Interaktive Container

```
docker run -it --name test ubuntu:latest
exit
docker start -it test
docker rm test
```

@[1](Startet einen neuen Ubuntu Container und verbindet auf dessen Console. Das Image wird automatisch runtergeladen.)
@[2](Der Container wird verlassen und beendet)
@[3](Startet den Container erneut)
@[4](Löscht den Container)

+++

### Einbinden von Volumes

+++

### Container im Hintergrund

```
docker create --name test ubuntu:latest server
docker start test
docker stop test
```

@[1](Erstellt einen Container, der beim Start das Programm `server` ausführt.)
@[2](Startet den Container. Der Container stoppt automatisch bei Programmende.)
@[3](Stoppt den Container.)

+++

### Freigeben von Ports

+++

### Logs einsehen

* Erzeugt das in einem Container laufende Programm Ausgaben, werden diese im Log gespeichert.
* `docker logs test`
  * Listet alle Log-Einträge des Containers mit Namen `test` auf.

+++

### Container verwalten

```
docker ps
docker ps -a
docker rm test
```

@[1](Es werden alle laufenden Container angezeigt)
@[2](Es werden alle Container angezeigt)
@[3](Löscht den Container mit Namen `test`. Ein Container ohne Namen kann über dessen ID adressiert werden.)

+++

### Images verwalten

+++

### Volumes verwalten

---

## Images erstellen
### Dockerfile, das Kochrezept

+++

### Dockerfile

+++

### Die wichtigsten Befehle

+++

### Build

---

## Infrastruktur
### Unterstützende Dienste in Cloud und on Premise

+++

### DockerHub

+++

### DockerCloud

+++

### DockerStore

+++

### Trusted Registry

+++

### Docker Data Center

---

## Swarm
### Skalierung durch Cluster

+++

### Was ist Swarm

+++

### Swarm verwalten

+++

### Services verwalten

---

## Stolpersteine
### Eigenarten und Wissenswertes

+++

### Bauen mit Cache

+++

### Signals vom Host

+++

### Persistenz im Schwarm

---

## Sicherheit

+++

### DockerDaemon benötigt root

+++

### User Namespaces