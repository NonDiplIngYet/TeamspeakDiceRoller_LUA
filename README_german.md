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

## Autoren

- **Alick | Alex**
- **Null-ARC | Fenrir**
**Viel Spaß mit fairen, deterministischen Würfelwürfen über alle deine liebsten TTRPGs!**

