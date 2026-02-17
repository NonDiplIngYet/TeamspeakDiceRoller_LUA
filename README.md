# TeamspeakDiceRoller - Refactored Merged Edition

A modular, maintainable Lua-based dice rolling utility designed for online roleplaying via TeamSpeak 3's Lua plugin. This edition merges contributions from both `alick-changes` and `nullArc-changes` and introduces a flattened, module-driven architecture for improved code organization and extensibility.

## Overview

**TeamspeakDiceRoller** is a specialized tool for tabletop RPG players using TeamSpeak 3 to coordinate gameplay. It provides deterministic, fair dice rolling across multiple game systems, with role-specific formatting, custom user colors, and system-agnostic generic rolls.

## Features

### Game Systems Supported

The tool supports dedicated rule implementations for:

- **DSA 4.1** (Das schwarze Auge 4.1) – German d20/d20/d20 skill system with critical successes/failures and talent points
- **Shadowrun 5** (SR5) – Pool-based d6 system with glitch detection and edge explosions
- **Call of Cthulhu (7th Edition)** – d100 percentile system with difficulty scaling and critical/fumble mechanics
- **KatharSys** (Degenesis Rebirth) – Modified pool system with automatic successes and trigger tracking
- **Blades in the Dark** (BitD) – d6 pool with critical success on multiple sixes
- **Powered by the Apocalypse** (PBtA) – 2d6 + modifier with tiered success thresholds
- **Generic Dice** – Flexible `XdY[±modifier]` notation for any system or ad-hoc rolls

### Core Capabilities

- **User Color System** – Assign custom colors or hex codes to players for chat visibility and personality
- **Color Presets** – Built-in color map (gold, blau/blue, grün/green, lila/purple, petrol, teal, rot/red, violett/violet, bordeaux, white)
- **Simple Rolls** – Quick one-command rolls: `!` (1d20), `?` (1d6), `!!` (1d100), `??` (1d66)
- **Owner Control** – Admin-only system activation, mode switching, and tool on/off
- **Statistics Check** – `!statcheck` rolls 100,000 dice per type to verify RNG distribution
- **Help System** – `!help` / `!hilfe` displays available commands

## Installation

### Requirements

- **TeamSpeak 3 Client** (version with Lua plugin support)
- **Lua Plugin** installed in TeamSpeak 3 (see [official Lua addon](https://www.myteamspeak.com/addons/L2FkZG9ucy8xZWE2ODBmZC1kZmQyLTQ5ZWYtYTI1OS03NGQyNzU5M2I4Njc%3D))
- **File system access** to TeamSpeak 3 plugins directory

### Installation Steps

1. **Open TeamSpeak 3 Client**

2. **Install Lua Plugin** (if not present)
   - Press `ALT+P` to open Options
   - Navigate to "Addons"
   - If Lua is not listed, download the latest version from the [official TeamSpeak addon site](https://www.myteamspeak.com/addons/L2FkZG9ucy8xZWE2ODBmZC1kZmQyLTQ5ZWYtYTI1OS03NGQyNzU5M2I4Njc%3D)

3. **Locate Plugin Directory**
   - **Windows**: `%APPDATA%\TS3Client\plugins\lua_plugin` (or `%LOCALAPPDATA%\TS3Client\plugins\lua_plugin` on older installs)
   - **Linux**: `~/.ts3client/plugins/lua_plugin`
   - **macOS**: `~/Library/Application Support/TeamSpeak 3/plugins/lua_plugin`

4. **Extract Release Package**
   - Extract the provided release `.zip` into the lua_plugin directory
   - You should now have a folder structure: `lua_plugin/roller/` containing:
     - `init.lua`
     - `events.lua`
     - `dice.lua`
     - `colors.lua`
     - `dsa.lua`
     - `systems/` (subdirectory with modular system handlers)

5. **Restart TeamSpeak 3**

6. **Verify Installation**
   - Press `ALT+P` → Addons → Lua (Settings)
   - Ensure only `"roller"` is checked
   - If other Lua modules are checked, the DiceRoller may conflict

## Usage

### Basic Commands (Always Active)

When the tool is enabled (`!on` / `!dice`), these commands work in any system mode:

| Command | Effect |
|---------|--------|
| `!` | Roll 1d20 |
| `?` | Roll 1d6 |
| `!!` | Roll 1d100 |
| `??` | Roll 1d66 (2d6 as two-digit hex number) |
| `!help` / `!hilfe` | Display available commands |
| `!farbe,<color_name>` | Set user color (e.g., `!farbe,gold` or `!farbe,#FF5733`) |

### System Activation (Owner Only)

| Command | System | Mode |
|---------|--------|------|
| `!on` / `!dice` | Tool Activation | Enables all dice rolling features |
| `!dsa` / `!dsa4` | DSA 4.1 | German d20/d20/d20 system |
| `!sr` / `!sr5` | Shadowrun 5 | Pool-based d6 system |
| `!coc` / `!call` | Call of Cthulhu | d100 percentile system |
| `!kat` / `!deg` | KatharSys | Modified pool d6 system |
| `!bitd` / `!blades` | Blades in the Dark | d6 pool system |
| `!pbta` / `!apoc` | Powered by the Apocalypse | 2d6 + modifier system |
| `!off` | Deactivate Tool | Disables all rolling |
| `!statcheck` | Statistics Verification | Test RNG with 100k rolls per die type |

### DSA 4.1 Mode

#### Attribute Check (1d20)
```
!<stat_value>                    # Single roll against attribute
!<stat_value>,<modifier>         # With bonus (+) or penalty (-)
```

#### Talent Check (3d20)
```
!<att1>,<att2>,<att3>,<skill_value>              # Full 3d20 check
!<att1>,<att2>,<att3>,<skill_value>,<modifier>   # With difficulty modifier
```

#### Dice Pool (d6)
```
?<number_of_dice>                # Roll Xd6 and sum
?<number_of_dice>,<modifier>     # Roll and add modifier
```

#### Special Rolls
```
!treffer                         # "Trefferzonenwurf" (hit zone roll)
```

### Shadowrun 5 Mode

```
!<pool_size>          # Roll XdY where 5+ count as success, 1s cause glitch
!<pool_size>,e        # With edge (exploding 6s on every die)
```

### Call of Cthulhu Mode

#### Skill Check
```
!<skill_value>                    # Roll d100 vs skill
!<skill_value>,<bonus_dice>       # With bonus dice (reroll under original tens)
!<skill_value>,<-malus_dice>      # With malus dice (take highest tens)
```

#### Generic Dice
```
?<num_dice>,<die_size>            # Roll XdY
?<num_dice>,<die_size>,<mod>      # Roll XdY + modifier
```

### KatharSys Mode

```
!<pool_size>          # Roll XdY (max 12; extras auto-succeed), 4+ count, 6 = trigger
```

### Blades in the Dark Mode

```
!<pool_size>          # Roll XdY, highest die determines outcome
```

### Powered by the Apocalypse Mode

```
!<modifier>           # Roll 2d6 + modifier
```

### Generic Rolls (Any System)

```
!<num>,<die>                # Total: roll XdY, sum result
!<num>,<die>,<modifier>     # Total with modifier
?<num>,<die>,<threshold>    # Pool: roll XdY, count successes at threshold+
```

## Project Structure

### Directory Layout

```
merged/
├── init.lua              # Module loader and TeamSpeak 3 event registration
├── events.lua            # Main dispatcher and event handler (flattened, low indentation)
├── dice.lua              # Core dice rolling functions (d4, d6, d8, d10, d12, d20, d100)
├── colors.lua            # User color management and color preset map
├── dsa.lua               # DSA 4.1 helper functions (attributes, criticals, result formatting)
├── systems/              # Modular system-specific handlers
│   ├── sr5.lua          # Shadowrun 5 processor
│   ├── coc.lua          # Call of Cthulhu processor
│   ├── kat.lua          # KatharSys processor
│   ├── bitd.lua         # Blades in the Dark processor
│   ├── pbta.lua         # Powered by the Apocalypse processor
│   └── generic.lua      # Generic dice roll fallback processor
├── changelog.md         # Release notes and version history
├── LICENSE              # MIT License
└── README.md            # This file
```

### Module Interfaces

Each system module in `systems/` exports a `process(message, fromName, dice)` function that returns `(response_string, should_send_boolean)`. This standardized interface allows the main dispatcher to treat all systems uniformly and easily add new game systems.

## Code Architecture

### Design Philosophy

This refactored edition prioritizes **readability and maintainability**:

- **Flattened dispatcher**: Early returns and guard clauses reduce indentation levels
- **Modular systems**: Each game system is isolated in its own file, avoiding "scope pyramid" antipattern
- **Reusable interfaces**: All system modules follow the same `process()` signature
- **Backward compatible**: Functionality and output are identical to the pre-refactored version

### Adding a New Game System

To add support for a new game system:

1. Create `merged/systems/mysystem.lua` with:
   ```lua
   local M = {}
   function M.process(message, fromName, dice)
       -- Parse message, call dice.rollDice(), format response
       return response_string, true  -- or false if unhandled
   end
   return M
   ```

2. In `merged/events.lua`, add to the `systems` table:
   ```lua
   local systems = {
       sr5 = require("roller/systems/sr5"),
       -- ... existing systems ...
       mysystem = require("roller/systems/mysystem"),
   }
   ```

3. Add system activation in the admin command handler:
   ```lua
   if message == "!mysys" then
       system = "mysystem"
       sendResponse(serverConnectionHandlerID, response .. "\n[b]My System[/b]")
       return
   end
   ```

## Dependencies

- **Lua 5.1+** (provided by TeamSpeak 3 Lua plugin)
- **TeamSpeak 3 API** (ts3, ts3defs) – provided by the plugin environment
- **No external Lua libraries** – all functionality is built-in or derived from std library

## Contributing

Contributions are welcome! To suggest improvements or report issues:

1. Test your changes thoroughly in a TeamSpeak 3 environment
2. Ensure backward compatibility with existing commands
3. Follow the existing code style and module pattern
4. Update both `README.md` and `README_german.md` if adding features
5. Submit changes via pull request or issue tracker

## License

MIT License – See [LICENSE](LICENSE) file for details.

## Authors

- **Alick | Alex** (DK_Alick) – Original DSA implementation, color system
- **Null-ARC | Fenrir** (nullArc-changes) – Extended systems (CoC, KatharSys, Blades, PBtA), generic rolls
- **Merged & Refactored** – Unified architecture, modular dispatcher, flattened control flow

## Support

For issues, questions, or feature requests:

1. Check the [MERGE_NOTES.md](MERGE_NOTES.md) for merge-specific details
2. Review the [changelog.md](changelog.md) for recent changes
3. Verify installation and system setup per the **Installation** section above

---

**Enjoy fair, deterministic dice rolls across all your favorite TTRPGs!**
