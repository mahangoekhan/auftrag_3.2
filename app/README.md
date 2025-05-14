# 📦 Auftrag 3.2 – Eigene HTML-Webseite mit Docker und NGINX

Dieses Projekt beschreibt, wie man eine eigene statische HTML-Webseite mit Docker und dem NGINX-Webserver betreibt. Die Seite wird aus einem lokalen Ordner `web/` in ein Docker-Image kopiert und dann über einen Container auf Port `8080` zugänglich gemacht.

---

## 🛠 Voraussetzungen

- Docker Desktop installiert (Windows, macOS oder Linux)
- Grundkenntnisse in der Kommandozeile (CMD oder PowerShell unter Windows)
- HTML-Dateien im Ordner `web/` im Projektverzeichnis

---

## 📁 Projektstruktur

```
html_docker_app_loesung/
│
├── app/
│   ├── Dockerfile.web
│   ├── web/
│   │   └── index.html  (und ggf. weitere HTML/CSS/JS-Dateien)
```

---

## 📝 Schritt 1: Dockerfile erstellen

In `app/Dockerfile.web` wird definiert, wie das Docker-Image aufgebaut wird.

```Dockerfile
# Dockerfile.web

# 1. Verwende das offizielle NGINX-Image als Basis
FROM nginx:latest

# 2. Kopiere alle Dateien aus dem lokalen web-Ordner in das Standardverzeichnis von NGINX
COPY web/ /usr/share/nginx/html/
```

### Erklärung:
- `FROM nginx:latest` holt das aktuellste NGINX-Image aus Docker Hub.
- `COPY web/ /usr/share/nginx/html/` kopiert die lokalen HTML-Dateien in den Ordner, den NGINX automatisch als Webroot verwendet.

---

## 🔨 Schritt 2: Image bauen

Führe im Verzeichnis `app/` den folgenden Befehl aus:

```bash
docker build -f Dockerfile.web -t auftrag_3_2_meine-webseite:v1 .
```

### Erklärung:
- `-f Dockerfile.web`: Gibt an, dass die Datei `Dockerfile.web` verwendet wird.
- `-t auftrag_3_2_meine-webseite:v1`: Gibt dem Image einen Namen (Tag).
- `.`: Der Punkt steht für das aktuelle Verzeichnis als Kontext, also wo Docker nach Dateien sucht.

✅ **Wichtig**: Achte darauf, dass der Ordner `web/` im aktuellen Verzeichnis vorhanden ist.

---

## ▶️ Schritt 3: Container starten

```bash
docker run -d -p 8080:80 --name webseite auftrag_3_2_meine-webseite:v1
```

### Erklärung:
- `-d`: Detached Mode – der Container läuft im Hintergrund.
- `-p 8080:80`: Leitet Port 8080 deines Host-Rechners auf Port 80 im Container weiter (NGINX Standardport).
- `--name webseite`: Gibt dem Container den Namen `webseite`.
- `auftrag_3_2_meine-webseite:v1`: Name des zuvor erstellten Images.

---

## 🌐 Webseite aufrufen

Öffne deinen Browser und gehe zu:

```
http://localhost:8080
```

Wenn alles geklappt hat, siehst du deine HTML-Webseite.

---

## 🧹 Nützliche Docker-Kommandos

| Aktion                        | Befehl                                                                 |
|-----------------------------|------------------------------------------------------------------------|
| Laufende Container anzeigen | `docker ps`                                                            |
| Alle Container anzeigen     | `docker ps -a`                                                         |
| Logs anzeigen               | `docker logs webseite`                                                |
| Container stoppen           | `docker stop webseite`                                                |
| Container löschen           | `docker rm webseite`                                                  |
| Image löschen               | `docker rmi auftrag_3_2_meine-webseite:v1`                            |
| Alle Images anzeigen        | `docker images`                                                       |

---

## ❌ Fehlerbehebung

| Problem                                           | Lösung                                                                 |
|--------------------------------------------------|------------------------------------------------------------------------|
| Webseite nicht erreichbar                        | Ist der Container gestartet? Port 8080 freigegeben?                   |
| Änderungen an HTML nicht sichtbar                | Image neu bauen und Container neu starten                            |
| Port 8080 ist schon belegt                       | Wähle einen anderen Port z.B. `-p 8081:80`                            |

---

## 📌 Hinweis zur Entwicklung

Wenn du oft Änderungen an HTML-Dateien machst, kannst du auch **Live Mounting** mit `-v` verwenden:

```bash
docker run -d -p 8080:80 --name webseite -v ${PWD}/web:/usr/share/nginx/html nginx
```

So sparst du dir das ständige Neubauen des Images.

---

## 🏁 Zusammenfassung

- Du hast ein Docker-Image auf Basis von NGINX gebaut.
- Deine HTML-Dateien werden in das Webverzeichnis des Containers kopiert.
- Mit einem Container läuft deine Webseite lokal über `localhost:8080`.

---

Viel Erfolg mit deiner eigenen Docker-Webseite! 🚀
