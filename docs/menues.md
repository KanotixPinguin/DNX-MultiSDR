# Menüs im Überblick

> Detailbeschreibungen aller Bedienelemente (Fenster-Titelleiste, ≡-Menü, Geräte-Einstellungen, Befehlsleiste, Statuszeile): **[bedienung.md](bedienung.md)**

## File (`File`)

![File-Menü](screens/27-menu-file.png)

- `Neuer Arbeitsbereich / Layout zurücksetzen` — kompletter Reset der Fensteranordnung
- `Arbeitsbereich-Datei öffnen…` — gespeichertes Layout laden (Datei wählen)
- `Arbeitsbereich speichern` / `speichern unter…` — Layout per Datei sichern
- `Arbeitsbereich JSON importieren…` / `exportieren` — Layout als JSON (z. B. für Austausch)

Kleiner Hinweistext im Menü: *„DNX-Menü: keine Workspace-Panels unten, alles als
kompakte Top-Menüs.“*

## View (`View`)

![View-Menü](screens/28-menu-view.png)

- `Ansicht / Anzeige` — Anzeigeoptionen
- `Vollbild umschalten` — Vollbild ein/aus
- `Alle minimieren` / `Alle wiederherstellen` / `Alle Fenster schließen`
- `Fenster kaskadieren` / `Fenster kacheln` — automatische Anordnungen

## Workspace (`Workspace`)

![Workspace-Menü](screens/29-menu-workspace.png)

- `Anordnung AN/AUS` — Raster-Anordnung umschalten (wie Toolbar-Button)
- **Arbeitsbereich 1–4** — Fenstergruppe wechseln (z. B. 1 = SAT, 2 = Wetter, 3 = Scan, 4 = Fax)
- **Move active RX to Arbeitsbereich 1–4** — aktiven RX in eine Gruppe verschieben
- `Unterfenster kaskadieren` / `kacheln` / `senkrecht stapeln` / `in Spalten stapeln`
- `Als Reiter anzeigen` — Fenster als Tabs statt freischwebend

## Preferences (`Preferences`)

![Preferences-Menü](screens/30-menu-preferences.png)

- `FFT Einstellungen` — öffnet/aktualisiert in dieser Beta das **FFT-Fenster** des
  aktiven RX (kein Einstellungs-Dialog; die FFT-Parameter stehen im Fenster selbst)
- `Geräte-Standardwerte` — in dieser Beta **ohne sichtbare Wirkung** (beobachtet)
- `Anzeige-Standardwerte für neue RX` — in dieser Beta **ohne sichtbare Wirkung** (beobachtet)
- `Audio-Equalizer aktiver RX` — 10-Band-LED-Equalizer des aktiven RX (Preamp, FLACH/BASS/MUSIK/SPRACHE/RESET)
- `Über DNX MultiSDR` — Version/Komponentenliste

Die pro-RX-Gerätekonfiguration (RF Gain, AGC, Samplerate, IF, Squelch, Bandplan …)
erreichst du über **`≡` in der Titelleiste des RX-Fensters → `Geräte-Einstellungen – RX <n>`**
— siehe [bedienung.md](bedienung.md) §5.

## Toolbar-Presets & Raster

Rechts neben `Raster: n` hat die Toolbar Fenstergrößen-Presets (`800 × 600` …
`5120 × 2880`) sowie Ansicht-Hilfen (`⛶ Viewport FIT`, `⟲ Reset`, `⚙ Device Defaults`,
`Anordnung: AN`). `Raster: n` steuert die Spaltenzahl der automatischen Anordnung —
passend zur Fenstergröße der RX-/Tool-Fenster (Standard 396 × 400 px).

Der Toolbar-Button **FFT** öffnet das folgende Analysen-Fenster:

## FFT — Spektrum & Wasserfall des aktiven RX

Der **FFT**-Button öffnet das Fenster **RX <n> / FFT / Spektrum** mit vier Reitern:
`Spektrum`, `Wasserfall`, `FFT`, `Skala`.

![FFT-Fenster](screens/41-fft-fenster.png)

*FFT-Fenster (RX 5 · DSP Test Source) mit `Spektrum`-Reiter: Darstellungsmodus `Linie`,
Spektrum-Palette `2511`, Wasserfall-Stil `Farbe`, `Spektrum-Tempo` 60 FPS,
`Linienfarbe`/`Füllfarbe oben`/`Füllfarbe unten` mit Hex-Werten + `AUS`-Schaltern,
`Füll-Deckkraft` 48 %, `Linien-Deckkraft` 1,00. Buttons `Max Hold löschen`,
`Persistenz löschen`, `Spektrum zurücksetzen`.*

`Viewport FIT` zeichnet Inhalt passend zur Fenstergröße, `Reset` stellt die
RX-Parameter auf Standard zurück (Gain, AGC, Empfangspegel). **`⚙ Device Defaults`**
öffnet in dieser Beta den Dialog *„Receiver hinzufügen“* (Geräteauswahl) — die echten
Vorgaben stecken in `≡ → Geräte-Einstellungen – RX <n>` (siehe [bedienung.md](bedienung.md) §5).
