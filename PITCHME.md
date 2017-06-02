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

+++

### Images

+++

### Container

+++

### Volumes

+++

### Netzwerk

---

## Lokale Nutzung
### Befehle zur Steuerung von Docker

+++

### Interaktive Container

+++

### Dienst-Container

+++

### Container verwalten

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