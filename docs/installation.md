# Installation & Betrieb — Docker, ohne Docker, Zusatz-Container

> **Wichtig vorweg:** Diese Veröffentlichung enthält **nur Dokumentation und
> Screenshots** — **keinen Anwendungscode, keine Images, keine Binaries, keinen
> Installer**. Es gibt daher *keinen* „Download und doppelklicken“-Weg, und es
> wird ihn mit diesem Release auch nicht geben. Diese Seite beschreibt, **wie das
> System aufgebaut ist und warum**, damit du den Betrieb verstehst. Sie ist keine
> Installationsanleitung für Dritte.

## Architektur-Überblick

DNX MultiSDR besteht aus einer **browserbasierten Web-Oberfläche** (UI, Fenster,
Karten, Tabellen, Sprachbefehle) und **spezialisierten Hintergrund-
Komponenten** für Empfang, DSP, Datenbanken und Assistenz. Die Komponenten
kommunizieren über lokale HTTP/WebSocket- und Binary-Kanäle.

### Betrieb mit Docker (Standard)

Der komplette Stack läuft bei mir als **Docker-Compose-Verbund** auf einem
Linux-Server (headless, ohne Desktop). Übersicht:

| Container | Basis (Beispiel) | Aufgabe |
|---|---|---|
| **dnx-web** | `caddy:2-alpine` | Liefert das Web-UI statisch aus (Caddy), terminiert HTTP/TLS (Port 80/443), Reverse-Proxy zu den Backends |
| **dnx-dsp** | eigener Build (`./rtl-worker`) | Der eigentliche **DSP-Worker**: Zugriff auf den **RTL-SDR** über `/dev/bus/usb`, Tuning, Gain/AGC, Streaming (Port 7355), speist alle RX-Fenster |
| **dnx-api** | eigener Build (uvicorn/FastAPI, `main_bb:app`) | **Band-Browser-/Datenbank-API** (Port 8000): Frequenzlisten, Welt-DB, Suche |
| **dnx-dab** | eigener Build (Python) | **DAB-Worker**: DAB/DAB+-Empfang und Demux für DAB-Fenster |
| **dnx-tetra** | eigener Build (Python `tetra_bridge.py`) | **TETRA-Bridge**: bindet TETRA-Demux/-Streaming ein (u. a. Kanalband, Audio-Export) |
| **dnx-ollama** | `ollama/ollama` | **Lokaler KI-Stack**: Sprach-/Assistenz-Features (Kommandos verstehen, Antworten), vollständig lokal — kein Cloud-Call nötig |

Gemeinsames Datenverzeichnis (`data/runtime`) für Konfiguration und
Laufzeitdaten zwischen `dnx-api`, `dnx-dsp` und `dnx-tetra`. Der DSP-Container
bekommt Zugriff auf den USB-Bus (`/dev/bus/usb`, rwm), damit er den RTL-SDR
benutzen kann — **das ist der Grund, warum Docker bei SDR-Setups bevorzugt
wird**: reproduzierbarer Aufbau, geringes Chaos, klare Trennung CPU/IO.

### Warum Zusatz-Container — der Zusammenhang

- **Trennung von Verantwortlichkeiten:** Der Browser ist ein schlechter Ort für
  USB-Zugriff und Echtzeit-DSP; der Caddy soll nur Web + TLS machen; die Datenbank
  soll nur Abfragen beantworten. Jeder Container hat genau eine Aufgabe und kann
  einzeln neu gebaut/gestartet werden.
- **Hardware-Nähe:** SDR-Sticks gehören in einen Container mit USB-Passthrough —
  Releases von Betriebssystem-Treibern tangieren die Web-UI nicht.
- **Ausfallsicherheit:** Fällt ein Dekoder-Worker aus (z. B. TETRA), läuft der
  Rest — der Fehler landet im `Fehler anzeigen`-Log, nicht in der Oberfläche.
- **Assistenz lokal:** Ollama läuft daneben im selben Verbund; Assistenz-
  Kommandos brauchen kein Internet und keine API-Keys.

### Betrieb ohne Docker (prinzipiell)

Die Architektur ist **nicht Docker-zwingend**:

- **Web-UI**: statische HTML/CSS/JS-Dateien auf jedem beliebigen Webserver
  (Caddy, nginx, Apache …) auslieferbar.
- **dnx-api**: Python + uvicorn auf jedem Linux/macOS; benötigt nur die
  Datenbank-Dateien und FastAPI/uvicorn.
- **dnx-dsp**: benötigt `libusb` + RTL-SDR-Treiber; auf jedem Linux nativ baubar
  (RTL-SDR ist seit Jahren Standard).
- **dnx-ollama**: optional; ohne Ollama entfallen nur die Assistenz-Features.

D. h.: Wer weiß, was er tut, kann alles nativ betreiben — nur **offiziell
abgestimmt und getestet ist der Docker-Weg**. Ohne den (geschlossenen) Code
ist das derzeit eh eine Fingerübung für Gleichgesinnte.

### Hardware-Voraussetzungen

- **RTL-SDR Blog V3** (RTL2838) — die von mir genutzte Antenne deckt **nur
  100 kHz – 30 MHz** ab, d. h. HF ist das Haupteinsatzgebiet; für VHF/UHF gilt
  das entsprechend nicht. Es gibt nur einen Stick (kein Dual-DSP).
- Optional: **Airspy R2** stattdessen.
- Eine **HF-Antenne** ist das A und O (Dummy-Loads lügen nicht, Langdrähte schon).
- Optional: HF-Konverter (z. B. 125–225 MHz ZF) für „echtes“ HF mit RTL.

## Hinweise zur Sicherheit

- Der DSP-Port (7355) und die API (8000) sollten **nicht offen ins Internet**
  gestellt werden — sie sind nicht gehärtet und sind nicht als öffentliche
  Dienste gedacht.
- HTTP (80) und HTTPS (443) werden beide von `dnx-web` terminiert.
- Alle Rohdaten bleiben lokal; einzig die eingestellten Cloud-APIs (Wetter-Kacheln,
  Blitze, open-meteo, OSM, Cesium-Ion) werden angesprochen — mit **deinen** eigenen
  Keys, lokal gespeichert.
