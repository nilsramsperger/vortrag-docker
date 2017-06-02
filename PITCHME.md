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
  * Beschränkung des Zugriffs auf Resourcen (cGroups)
  * Sandboxing (Kernel Namespaces)
  * Eigene Netzwerk-Stacks je Container
* Verringerter Overhead durch Wegfall des Gastsystems
  * Verringerter Resourcenverbrauch
  * Schnelles Starten und Stoppen von Containern

---

## Wie funktioniert Docker

---

## Lokale Nutzung

---

## Images aus Dockerfiles

---

## Infrastruktur

---

## Swarm

---

## Stolpersteine

---

## Sicherheit