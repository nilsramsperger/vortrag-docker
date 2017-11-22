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

* Instanz eines Image
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
* Sonderformen
  * Named Volumes
  * Host Volumes

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

@[1](Startet einen neuen Ubuntu Container und verbindet auf dessen Console)
@[2](Der Container wird verlassen und beendet)
@[3](Startet den Container erneut)
@[4](Löscht den Container)

+++

### Einbinden von Volumes

```
docker run -it --name test -v /dir1:/dir2 ubuntu:latest
docker run -it --name test -v volName:/dir2 ubuntu:latest
```

@[1](Bindet ein Verzeichnis vom Host ein)
@[2](Bindet ein Named Volume ein)

+++

### Container im Hintergrund

```
docker create --name test ubuntu:latest server
docker start test
docker stop test
```

@[1](Erstellt einen Container und führt nach dessen Start einen Befehl aus)
@[2](Startet den Container)
@[3](Stoppt den Container)

+++

### Freigeben von Ports

```
docker create --name test -p 8080:80 ubuntu:latest
```

@[1](Bindet den Port 80 des Containers an Port 8080 des Host)

+++

### Logs einsehen

* Erzeugt das in einem Container primär laufende Programm _(PID 1)_ Ausgaben, werden diese im Log gespeichert.
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
@[3](Löscht den Container)

+++

### Images verwalten

```
docker images
docker rmi imageName
docker pull imageName
```

@[1](Listet alle lokalen Images auf)
@[2](Löscht ein Image)
@[3](Lädt ein Image von DockerHub)

+++

### Volumes verwalten

```
docker volume ls
docker volume rm volName
```

@[1](Listet alle Volumes auf)
@[2](Löscht ein Volume)

---

## Images erstellen
### Dockerfile, das Kochrezept

+++

### Manuelle Erstellung

```
docker run -it --name baustelle ubuntu:latest
apt-get install ...
exit
docker commit baustelle namespace/meinimage:latest
docker run -it --name test namespace/meinimage:latest
```

@[1](Startet einen interaktiven Container)
@[2](Beliebige Befehle im Container ausführen um benötigte Programme zu installieren und einzurichten)
@[3](Beendet den Container)
@[4](Speichert den Container als Image)
@[5](Erstellt einen neuen interaktiven Container aus dem erstellten Image)

+++

### Dockerfile

* Beschreibt den schrittweisen Aufbau des Image
* Bestandteile
  * Base Image
  * Befehle zum Aufbau des Image
    * Ausführen von Befehlen
    * Kopieren von Dateien
  * Konfiguration des Image
    * Volumes
    * Ports
    * Startbefehl

+++

### Die wichtigsten Befehle

```
FROM ubuntu:latest
RUN apt-get update && apt-get install nginx
ADD /hostPfad/quelle /containerPfad/ziel
VOLUME /pfad
EXPOSE 8080
CMD ["nginx"]
```

@[1](Legt das zu verwendende BaseImage fest)
@[2](Führt einen Befehl im Container aus)
@[3](Kopiert die Datei vom Host in den Container)
@[4](Legt für einen Ordner ein Volume an)
@[5](Der Port 8080 des Containers wird freigegeben)
@[6](Der beim Containerstart auszuführende Befehl)

+++

### Build

```
docker build -t maxmustermann/nginx:latest /pfad
docker build -t maxmustermann/nginx:latest .
docker build --no-cache -t maxmustermann/nginx:latest .
```

@[1](Baut ein Image mit dem gegebenen Namen aus einem Dockerfile am gegebenen Pfad)
@[2](Bauen im Verzeichnis des Dockerfiles)
@[3](Bauen ohne Cache)

---

## Infrastruktur
### Unterstützende Dienste in Cloud und on Premise

+++

### DockerHub

* Registry für Images
* Automatische Builds

+++

### DockerCloud

* Registry für Images
* Automatische Builds
* Verwaltung von Docker Swarm

+++

### DockerStore

* Frontend zur Suche von Images
* Quellen
  * DockerHub (freie Images)
  * Kommerzielle Images

+++

### Trusted Registry

* Registry für Images
* On Premise 

---

## Stolpersteine
### Eigenarten und Wissenswertes

+++

### Bauen mit Cache

* Jeder Befehl im Dockerfile wird als deterministisch betrachtet
* Das Ergebnis eines Befehls wird gecached
  * Problem bei nicht deterministischen Befehlen
* Lösung
  * Befehle mit `&&` verketten. z.B.: `RUN apt-get update && apt-get install nginx`
  * `docker build` mit Parameter `--no-cache` aufrufen

+++

### Signals vom Host

* Container wird vom Host gestoppt
* Der Prozess mit PID 1 im Container erhält SIGTERM
* Laufen mehrere Prozesse im Container, muss der mit PID 1 diese beenden
* Lösung
  * Supervisor Script
  * Startet und Stoppt alle benötigten Prozesse
  
+++

### Container Namen im Netzwerk

* Container werden über Namen adressiert
* Contianer Namen sind case sensitive
* ULRs sind **nicht** case sensitive
* Container Namen müssen vollständig klein geschrieben werden
  * Ansonsten kann es zu Problemen mit der Ereichbarkeit kommen

---

## Sicherheit

+++

### Docker Engine benötigt root

* Docker greift tief in Host System ein, daher wird root Recht benötigt
* Docker kann über Socket gesteuert werden
  * Container mit Zugriff darauf können theoretisch auf dem Host mit root Recht agieren
* Initialer Prozess im Container läuft als root
  * Weitere Prozesse sollten unter Funktionsusern gestartet werden
  * User Namespaces verwenden

+++

### User Namespaces

* Mapping von Usern
  * Standard User auf Host
  * root im Container
* Kein root Recht auf Host, im Fall eines Ausbruchs aus dem Container
* User Namaspaces müssen explizit aktiviert werden! 