# TeamspeakDiceRoller – Refaktorierte Merged Edition

Ein modulares, wartbares Lua-basiertes Würfelwurf-Tool für Online-Rollenspiele über das Lua-Plugin von TeamSpeak 3. Diese Edition vereinigt Beiträge aus `alick-changes` und `nullArc-changes` und führt eine abgeflachte, modulgetriebene Architektur für verbesserte Code-Organisa­tion und Erweiterbarkeit ein.

## Überblick

**TeamspeakDiceRoller** ist ein spezialisiertes Werkzeug für Tabletop-RPG-Spieler, die TeamSpeak 3 zur Spielkoordination nutzen. Es bietet deterministische Würfelwürfe über mehrere Spielsysteme hinweg, mit rollentypischer Formatierung, benutzerdefinierten Spielerfarben und systemagnostischen generischen Würfeln.

## Funktionen

### Unterstützte Spielsysteme

Das Tool bietet dedizierte Regelimplementierungen für:

- **DSA 4.1** (Das schwarze Auge 4.1) – Deutsches d20/d20/d20-System mit kritischen Erfolgen/Misserfolgen und Talentwerten
- **Shadowrun 5** (SR5) – Poolbasiertes d6-System mit Glitch-Erkennung und kritischen Sechsen
- **Call of Cthulhu (7. Edition)** – d100-Perzentil-System mit Schwierigkeitsskalierung und kritischen/Patzer-Mechaniken
- **KatharSys** (Degenesis Rebirth) – Modifiziertes Pool-System mit automatischen Erfolgen und Trigger-Tracking
- **Blades in the Dark** (BitD) – d6-Pool mit kritischem Erfolg bei mehreren Sechsen
- **Powered by the Apocalypse** (PBtA) – 2d6 + Modifikator mit gestuften Erfolgsschwellen
- **Generische Würfel** – Flexible Notation `XdY[±Modifikator]` für jedes System

### Kernfähigkeiten

- **Benutzer-Farbsystem** – Weise Spielern benutzerdefinierte Farben oder Hex-Codes zu für Chat-Sichtbarkeit und Persönlichkeit
- **Farbvoreinstellungen** – Eingebaute Farbzuordnung (gold, blau, grün, lila, petrol, teal, rot, violett, bordeaux, white)
- **Schnelle Würfe** – Einteilige Würfelbefehle: `!` (1W20), `?` (1W6), `!!` (1W100), `??` (1W66)
- **Besitzerkontrolle** – Nur Admin kann System aktivieren, Modus wechseln und Tool an/aus schalten
- **Statistik-Check** – `!statcheck` wertet 100.000 Würfel pro Typ, um Zufallsverteilung zu verifizieren
- **Hilfesystem** – `!help` / `!hilfe` zeigt verfügbare Befehle

## Installation

### Voraussetzungen

- **TeamSpeak 3 Client** (Version mit Lua-Plugin-Unterstützung)
- **Lua-Plugin** in TeamSpeak 3 installiert (siehe [offizielles Lua-Add-on](https://www.myteamspeak.com/addons/L2FkZG9ucy8xZWE2ODBmZC1kZmQyLTQ5ZWYtYTI1OS03NGQyNzU5M2I4Njc%3D))
- **Dateisystem-Zugriff** auf TeamSpeak 3 Plugin-Verzeichnis

### Installationsschritte

1. **Öffne TeamSpeak 3 Client**

2. **Installiere Lua-Plugin** (falls nicht vorhanden)
   - Drücke `ALT+P`, um Optionen zu öffnen
   - Navigiere zu "Add-ons"
   - Falls Lua nicht aufgelistet ist, lade die neueste Version von der [offiziellen TeamSpeak-Site](https://www.myteamspeak.com/addons/L2FkZG9ucy8xZWE2ODBmZC1kZmQyLTQ5ZWYtYTI1OS03NGQyNzU5M2I4Njc%3D) herunter

3. **Lokalisiere Plugin-Verzeichnis**
   - **Windows**: `%APPDATA%\TS3Client\plugins\lua_plugin` (oder `%LOCALAPPDATA%\TS3Client\plugins\lua_plugin` bei älteren Installationen)
   - **Linux**: `~/.ts3client/plugins/lua_plugin`
   - **macOS**: `~/Library/Application Support/TeamSpeak 3/plugins/lua_plugin`

4. **Extrahiere Release-Paket**
   - Extrahiere die bereitgestellte `.zip` in das lua_plugin-Verzeichnis
   - Du solltest nun folgende Ordnerstruktur haben: `lua_plugin/roller/` mit:
     - `init.lua`
     - `events.lua`
     - `dice.lua`
     - `colors.lua`
     - `dsa.lua`
     - `systems/` (Unterverzeichnis mit modularen System-Handlern)

5. **Starte TeamSpeak 3 neu**

6. **Verifiziere Installation**
   - Drücke `ALT+P` → Add-ons → Lua (Einstellungen)
   - Stelle sicher, dass nur `"roller"` aktiviert ist
   - Falls andere Lua-Module aktiviert sind, kann der DiceRoller in Konflikt geraten

## Verwendung

### Grundbefehle (Immer aktiv)

Wenn das Tool aktiviert ist (`!on` / `!dice`), funktionieren diese Befehle in jedem Systemmodus:

| Befehl | Effekt |
|--------|--------|
| `!` | Würfle 1W20 |
| `?` | Würfle 1W6 |
| `!!` | Würfle 1W100 |
| `??` | Würfle 1W66 (2W6 als zweistellige Hexalzahl) |
| `!help` / `!hilfe` | Zeige verfügbare Befehle an |
| `!farbe,<farbname>` | Setze Benutzerfarbe (z.B. `!farbe,gold` oder `!farbe,#FF5733`) |

### System-Aktivierung (Nur Besitzer)

| Befehl | System | Modus |
|--------|--------|-------|
| `!on` / `!dice` | Tool-Aktivierung | Aktiviert alle Würfel-Funktionen |
| `!dsa` / `!dsa4` | DSA 4.1 | Deutsches d20/d20/d20-System |
| `!sr` / `!sr5` | Shadowrun 5 | Pool-basiertes d6-System |
| `!coc` / `!call` | Call of Cthulhu | d100-Perzentil-System |
| `!kat` / `!deg` | KatharSys | Modifiziertes Pool-System |
| `!bitd` / `!blades` | Blades in the Dark | d6-Pool-System |
| `!pbta` / `!apoc` | Powered by the Apocalypse | 2d6 + Modifikator-System |
| `!off` | Tool deaktivieren | Deaktiviert alle Würfelrollen |
| `!statcheck` | Statistik-Verifizierung | Teste RNG mit 100k Würfeln pro Würfeltyp |

### DSA 4.1 Modus

#### Eigenschaftsprobe (1W20)
```
!<attributswert>                 # Einfacher Wurf gegen Attribut
!<attributswert>,<modifikator>   # Mit Boni (+) oder Mali (-)
```

#### Talentprobe (3W20)
```
!<att1>,<att2>,<att3>,<talentwert>                # Vollständig 3W20-Probe
!<att1>,<att2>,<att3>,<talentwert>,<modifikator>  # Mit Schwierigkeitsmodifikator
```

#### Würfelpool (W6)
```
?<anzahl_würfel>                 # Werfe XW6 und summiere
?<anzahl_würfel>,<modifikator>   # Werfe und addiere Modifikator
```

#### Spezialwürfe
```
!treffer                         # "Trefferzonenwurf"
```

### Shadowrun 5 Modus

```
!<pool_größe>       # Werfe XdY, wo 5+ als Erfolg zählt, 1en versursachen Glitch
!<pool_größe>,e     # Mit Edge (explodierende 6en auf jedem Würfel)
```

### Call of Cthulhu Modus

#### Fertigkeit-Check
```
!<fertigkeit_wert>                    # Werfe W100 gegen Fertigkeit
!<fertigkeit_wert>,<bonus_würfel>     # Mit Bonus-Würfeln (Neuroll unter Original-Zehnern)
!<fertigkeit_wert>,<-malus_würfel>    # Mit Malus-Würfeln (nimm höchste Zehner)
```

#### Generische Würfel
```
?<num_würfel>,<würfeltyp>         # Werfe XdY
?<num_würfel>,<würfeltyp>,<mod>   # Werfe XdY + Modifikator
```

### KatharSys Modus

```
!<pool_größe>       # Werfe XdY (max. 12; Extras automatisch Erfolg), 4+ zählt, 6 = Trigger
```

### Blades in the Dark Modus

```
!<pool_größe>       # Werfe XdY, höchster Würfel bestimmt Ausgang
```

### Powered by the Apocalypse Modus

```
!<modifikator>      # Werfe 2W6 + Modifikator
```

### Generische Würfe (Beliebiges System)

```
!<num>,<würfel>                 # Summe: Werfe XdY, summiere Ergebnis
!<num>,<würfel>,<modifikator>   # Summe mit Modifikator
?<num>,<würfel>,<schwelle>      # Pool: Werfe XdY, zähle Erfolge ab Schwelle+
```

## Projektstruktur

### Verzeichnislayout

```
merged/
├── init.lua              # Module-Loader und TeamSpeak 3 Event-Registrierung
├── events.lua            # Haupt-Dispatcher und Event-Handler (abgeflacht, wenig Einrückung)
├── dice.lua              # Kern-Würfelfunktionen (W4, W6, W8, W10, W12, W20, W100)
├── colors.lua            # Benutzer-Farverwaltung und Farbvoreinstellungen-Zuordnung
├── dsa.lua               # DSA 4.1 Hilfsfunktionen (Attribute, Kritische, Ergebnisformatierung)
├── systems/              # Modulare sistem-spezifische Handler
│   ├── sr5.lua          # Shadowrun 5 Prozessor
│   ├── coc.lua          # Call of Cthulhu Prozessor
│   ├── kat.lua          # KatharSys Prozessor
│   ├── bitd.lua         # Blades in the Dark Prozessor
│   ├── pbta.lua         # Powered by the Apocalypse Prozessor
│   └── generic.lua      # Generischer Würfel-Fallback-Prozessor
├── changelog.md         # Versionshinweise und Verlauf
├── LICENSE              # MIT-Lizenz
└── README.md            # Diese Datei
```

### Modul-Schnittstellen

Jedes System-Modul in `systems/` exportiert eine `process(message, fromName, dice)`-Funktion, die `(antwort_string, sollte_senden_boolean)` zurückgibt. Diese standardisierte Schnittstelle ermöglicht es dem Haupt-Dispatcher, alle Systeme einheitlich zu behandeln und neue Spielsysteme leicht hinzuzufügen.

## Code-Architektur

### Designphilosophie

Diese refaktorierte Edition priorisiert **Lesbarkeit und Wartbarkeit**:

- **Abgeflachter Dispatcher**: Frühe Returns und Guard-Klauseln reduzieren Einrückungsstufen
- **Modulare Systeme**: Jedes Spielsystem ist in einer eigenen Datei isoliert und vermeidet das "Scope Pyramid"-Antipattern
- **Wiederverwendbare Schnittstellen**: Alle System-Module folgen der gleichen `process()`-Signatur
- **Rückwärts kompatibel**: Funktionalität und Ausgabe sind mit der vor-refaktorierten Version identisch

### Ein neues Spielsystem hinzufügen

Um Unterstützung für ein neues Spielsystem hinzuzufügen:

1. Erstelle `merged/systems/meinsystem.lua` mit:
   ```lua
   local M = {}
   function M.process(message, fromName, dice)
       -- Parse Nachricht, rufe dice.rollDice() auf, formatiere Antwort
       return antwort_string, true  -- oder false wenn nicht verarbeitet
   end
   return M
   ```

2. In `merged/events.lua`, füge zur `systems`-Tabelle hinzu:
   ```lua
   local systems = {
       sr5 = require("roller/systems/sr5"),
       -- ... bestehende Systeme ...
       meinsystem = require("roller/systems/meinsystem"),
   }
   ```

3. Füge System-Aktivierung im Admin-Befehls-Handler hinzu:
   ```lua
   if message == "!meinsys" then
       system = "meinsystem"
       sendResponse(serverConnectionHandlerID, response .. "\n[b]Mein System[/b]")
       return
   end
   ```

## Abhängigkeiten

- **Lua 5.1+** (bereitgestellt durch TeamSpeak 3 Lua-Plugin)
- **TeamSpeak 3 API** (ts3, ts3defs) – bereitgestellt durch die Plugin-Umgebung
- **Keine externen Lua-Bibliotheken** – alle Funktionalität ist eingebaut oder abgeleitet von Standardbibliothek

## Mitwirkung

Mitwirkungen sind willkommen! Um Verbesserungen vorzuschlagen oder Probleme zu melden:

1. Teste deine Änderungen gründlich in einer TeamSpeak 3-Umgebung
2. Stelle sicher, dass Rückwärtskompatibilität mit vorhandenen Befehlen gegeben ist
3. Folge dem vorhandenen Code-Stil und Modul-Pattern
4. Aktualisiere beide `README.md` und `README_german.md` bei neuen Funktionen
5. Reiche Änderungen über Pull-Request oder Issue-Tracker ein

## Lizenz

MIT-Lizenz – Siehe [LICENSE](LICENSE)-Datei für Details.

## Autoren

- **Alick | Alex** (DK_Alick) – Ursprüngliche DSA-Implementierung, Farbsystem
- **Null-ARC | Fenrir** (nullArc-changes) – Erweiterte Systeme (CoC, KatharSys, Blades, PBtA), generische Würfe
- **Merged & Refactored** – Vereinigte Architektur, modularer Dispatcher, abgeflachte Kontrollfluss

## Support

Bei Problemen, Fragen oder Funktionsanfragen:

1. Prüfe [MERGE_NOTES.md](MERGE_NOTES.md) für Merge-spezifische Details
2. Überprüfe [changelog.md](changelog.md) auf neueste Änderungen
3. Verifiziere Installation und System-Setup pro **Installation** oben

---

**Viel Spaß mit fairen, deterministischen Würfelwürfen über alle deine liebsten TTRPGs!**
