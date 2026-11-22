# Häufige Fragen (FAQ)

## Ich will das selbst ausprobieren — wo bekomme ich es her?

Diese Veröffentlichung enthält **nur Dokumentation und Screenshots**, keine Software.
Es gibt (noch) keinen öffentlichen Download, kein Image, kein Release mit Code.
Wer etwas Ähnliches bauen will, findet in [installation.md](installation.md) und
[architektur.md](architektur.md) die nötige Landkarte. Für den Rest gilt: E-Mail/Nachricht
mit Geduld — Support ist Glückssache, siehe [disclaimer.md](disclaimer.md).

## Brauche ich für die Wetterzentrale eigene API-Schlüssel?

Ja, sinnvoll sind sie: **Cesium Ion** (3D-Karten, 3D-Orbit/3D-Weltall/WZ-Karte 3D),
**OpenWeatherMap/Mapbox/Bing/ArcGIS/MapTiler/Google Maps/CheckWX** je nach gewünschter
Karte/Auflösung. Ohne Keys sind die offenen Wege (DWD-Warnungen/-Radar/-Blitze, OSM,
RainViewer, Open-Meteo) verfügbar, aber mit Limits. Alle Schlüssel werden nur lokal im
Browser gespeichert.

## Warum zeigen die Screenshots "Musterstadt" und unscharfe Bereiche?

Datenschutz. Für die Veröffentlichung wurden persönliche Standortangaben (Ort, PLZ,
Koordinaten) ersetzt bzw. weichgezeichnet und der Positionsmarker in den Karten
ausgeblendet. Die Funktionen sind davon unberührt.

## UTC oder MEZ — was gilt für HF-Fax-Sendezeiten?

Sendepläne (DWD, BBC, NOAA …) sind **immer UTC**. DNX MultiSDR rechnet auf Wunsch in
deine festgelegte Ortszeit um (`Ortszeit für die Sendezeiten`-Dialog beim ersten Öffnen).
Werkseinstellung ist UTC; MEZ/Berlin/eigene Ortsauswahl wählbar. Winterzeit: MEZ = UTC+1,
Sommerzeit: MESZ = UTC+2. Wer mit dem Papier-Sendeplan arbeitet, kommt mit UTC am
schnellsten ans Ziel — der Unterschied hat mir selbst schon eine Sendelücke verdorben.

## Warum öffnet `+ Add Channel` bei EAS/Radiosonde/HF-Fax den Receiver-Dialog?

Weil diese Module **an einen Empfänger gebunden** sind: Sie erzeugen keine eigenständigen
Kartendienste, sondern dekodieren Signale eines RX. Ohne aktiven RX fragt der Dialog nach
einer Quelle (RTL-SDR, Airspy, DSP Test Source oder Remote). Mit einem RX öffnen sich die
Modul-Fenster daneben (siehe Screenshots in [add-channel.md](add-channel.md#rx--wetterdecoder)).

## Läuft das auch ohne RTL-SDR?

Die Oberfläche läuft komplett, die meisten Datenbanken/Karten/Wettermodule auch —
für Live-Funk braucht es eine Quelle: RTL-SDR (empfohlen), Airspy, oder `DSP Test Source`
zum reinen Kennenlernen (synthetisches Signal).

## Benötigt die App Internet?

Für Karten, Wetterdaten, Blitzlisten, TLE/Orbit und Suchfunktionen: ja. Für reinen
Empfang/Dekodieren lokaler Funk: nein (bis auf Zeitsynchronisation und einmalige
Quellen-Downloads).

## Wie sieht es mit dem Standort aus?

Der Standort kommt vom DNX-Server (`Ort ermitteln: vom Server (nur gelieferte …)`) oder
wird manuell per `Ort/PLZ suchen`/`Mein Standort` gesetzt und nur lokal gespeichert —
er wird nicht an Dritte übertragen außer an die eingestellten Karten-APIs (bei Bedarf).

## Kann ich Fehler melden?

Ja, aber ohne Anspruch auf Bearbeitung. Beschreibe möglichst präzise: was du getan
hast, was du erwartet hast, was passiert ist — idealerweise mit Log-Auszug aus
`Fehler anzeigen`. Reaktion erfolgt nach Zeit und Lust.

## Darf ich das forken/ändern/veröffentlichen?

Für die **Dokumentation**: ja, MIT — siehe [LICENSE](../LICENSE). Für die Anwendung:
nicht Teil dieser Veröffentlichung; siehe Disclaimer.

## Warum erstellt jemand sowas?

Weil Funk, Wetter und Satelliten faszinierend sind — und weil niemand auf diese
Mischung gewartet hat. Genau deshalb ist es frei, und genau deshalb gibt es keinen Support.
