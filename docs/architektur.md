# Warum nicht alles in JavaScript?

Eine Frage, die ich mir beim Bauen oft gestellt habe — und die ehrlich beantwortet werden muss:

> **Warum ist nicht ALLES JavaScript?**

Kurz gesagt: Weil der Browser der schlechteste Platz der Welt ist, um Funk zu machen —
aber der beste, um ihn zu bedienen.

## Was JavaScript nicht kann — und warum das egal ist

**1. USB & Echtzeit.** Ein RTL-SDR spricht über `libusb`, schreibt und liest mit
Mega-Samples pro Sekunde. Browser erlauben das prinzipiell nicht — `WebUSB` existiert,
ist aber für SDR-Workloads ungeeignet (Latenz, Treiberkonflikte, Berechtigungschaos),
und außerdem bräuchte es dann RTL-Treiber-Neueimplementierungen in JS. Das wäre ein
Projekt für sich — mit schlechterem Timing und ohne Wartbarkeit.

**2. DSP in Echtzeit.** Ein RTL-SDR liefert ~2,4 MS/s Rohdaten. FFT, Demodulation,
De-Emphasis, Filterbänke, Rausch-Handling, Synchronisierung (TDMA bei DMR/TETRA!) —
das sind numerisch harte Aufgaben mit harten Deadlines. Ein nicht-Echtzeit-OS mit
Browser-Sandbox dazwischen ist der Garant für Dropouts beim Dekodieren. Der DSP gehört
in einen nativen Prozess mit direktem Speicherzugriff.

**3. WASM ist nett, aber kein Freifahrtschein.** Ja, WebAssembly könnte Teile
beschleunigen — aber der Flaschenhals bleibt der Datenzugriff, nicht die CPU-Bits.
Und: WASM ersetzt nicht die fehlenden Treiber.

**4. Lizenz-Hygiene.** Viele gute Decoder (DSD-Familien, native HF-Fax- und Wetterfax-
Demodulatoren, SAT-Ephemeriden aus professionellen Bibliotheken) sind C/C++ und
teilweise GPL. Sie auf Server-Seite zu bündeln ist rechtlich sauber; als JS-Rewrites
auszuliefern würde mich und jeden Nutzer in Lizenzfragen ziehen. So bleiben die
nativen Teile nativ — und die Web-UI darf frei sein.

**5. Ausfallsicherheit & Multiclient.** Läuft der DSP nativ, können mehrere Clients
(Web-UI, Remote-RX, …) auf dieselbe Hardware zugreifen und sich den Strom teilen —
nicht jeder Browser-Tab kämpft allein um den USB-Stick.

## Was JavaScript **sehr** gut kann — und warum das gerne genutzt wird

- **UI & Fenster-Management** — Raster, kaskadieren, Tabs, Docking: Browser sind UI-Könige.
- **Karten** — Leaflet + OpenStreetMap, Cesium 3D-Globe, Orbital-Pfade: WebGL reine Stärke.
- **Tabellen & Spektrum-Visualisierung** — Canvas, 60 fps Wasserfall, dynamisches Re-Rendering.
- **Sprach-Features** — `SpeechSynthesis` (Ansagen) und `SpeechRecognition` (Befehle) sind im Browser komplett frei Haus.
- **Kommandozeilen-Feeling** — die untere Befehlsleiste (`oeffne AIS | wetter | scan …`) parse ich bequem in JS.

## Das Ergebnis

"JavaScript für alles, was man sehen und anfassen kann — native Prozesse für alles,
was man hören und dekodieren muss."

Die App fühlt sich an wie eine native Anwendung, weil sie es unter der Haube teilweise ist.
Der Browser ist nur die Schale — die läuft lokal im LAN/Netz, keine Cloud, kein Account,
kein Abo.

## Und wie kam das alles zustande?

Ehrlich: **zum großen Teil mit KI.** DNX MultiSDR ist über Monate in Dialog mit KI-
Assistenzen entstanden — jede Komponente wurde gemeinsam entworfen, programmiert,
verworfen, neu gebaut und getestet. Der Code ist entsprechend heterogen, nicht immer
hübsch und nie "generiert und fertig". Er funktioniert — manches davon besser, als
man es von einem Feierabendprojekt erwarten dürfte. Und manchmal eben auch nicht;
dafür ist der `Fehler anzeigen`-Button da.

## Was NICHT veröffentlicht wird — und warum

Aus denselben Gründen (Lizenz, Qualität, Verantwortung) bleibt der Anwendungscode
(aktuell) privat. Diese Veröffentlichung ist bewusst eine **Dokumentations-Beta**:
Sie zeigt, was gebaut wurde, wie es aussieht und wie man damit umgeht — **ohne** den
Hintergrund-Automaten offenzulegen. Wer ähnliches will, kann es mit den
ARCHITEKTUR-Hinweisen hier selbst in die Hand nehmen — oder abwarten, ob das Projekt
später ganz (oder teilweise) offen wird.
