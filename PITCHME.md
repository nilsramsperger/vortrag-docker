### Die wichtigsten Befehle

```dockerfile
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
