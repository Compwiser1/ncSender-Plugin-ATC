# ncSender Plugin - ATC

**Version**: 0.1.64
**Category**: Tool Changer
**Requirements**: ncSender 2.0.0+ (OSS) or ncSender Pro 2.0.0+

Automatic tool changer support for **RapidChange ATC** systems in ncSender — automated `M6` tool change sequences, tool length setter integration, and a full configuration dialog from the Tools menu.

> **IMPORTANT DISCLAIMER:** This plugin drives real machine motion, including spindle starts and Z moves toward a tool magazine. If you choose to use it, you do so entirely at your own risk. The maintainers are not responsible for any damage, malfunction, or personal injury that may result from its use or misuse. Verify every position setting and dry-run before cutting.

---

## 📦 About this repository

This is a continuation fork of [siganberg/ncsender-plugin-rapidchangeatc](https://github.com/siganberg/ncsender-plugin-rapidchangeatc), originally created by Francis Marasigan, preserved and maintained here so the plugin keeps working after the upstream repository goes away. The full upstream commit history is retained in this repo.

The plugin id is unchanged (`com.ncsender.rapidchangeatc`), so an existing install keeps its saved settings when it is updated from this repo. Only one copy of the plugin should be enabled at a time.

---

## 🎯 What it does

### Automatic tool change
- Automated `M6` tool change sequences for multi-pocket ATC systems
- Support for 1–8 tool pockets
- Configurable pocket orientation (X or Y axis) and direction
- Automatic pocket position calculation from pocket distance
- Skips the change entirely when the requested tool is already loaded

### Tool length setter integration
- Automated tool length probing via the `$TLS` command
- Configurable tool setter X/Y location and probe parameters (seek distance, feedrate)
- Automatic tool offset management via `G43.1`
- Per-tool TLS offsets pulled from the ncSender Tool Library
- Optional automatic TLS after the first `$H` (home)
- Multiple sensor options (Probe/TLS or Aux ports)

### RapidChange ATC models
- **Basic** — standard ATC functionality
- **Pro** — adds spindle-at-speed support
- **Premium** — full features including dust cover commands

### Collet sizes
ER11, ER16, ER20, ER25, ER32 — RPM and Z retreat defaults are applied automatically per collet size.

### Probe tool (tool 99)
Optional dedicated probe tool with custom load/unload G-code, handled separately from regular tools.

### Safety
- Modal, non-closable dialogs during critical operations, each with **Abort** / **Continue**
- Operator prompts appear only after machine motion has fully stopped
- Optional spindle-at-speed verification
- Configurable ATC start delay

---

## ⌨️ Supported commands

| Command | Description |
|---------|-------------|
| `M6 Tx` | Perform an automatic tool change to pocket x |
| `$TLS` | Run the tool length setter routine |
| `$POCKET1` | Move to the pocket 1 position |
| `$H` | Home the machine (with optional automatic TLS if a tool is loaded) |

```gcode
; Automatic tool change to tool 3
M6 T3

; Manual tool length measurement
$TLS

; Move to pocket 1
$POCKET1

; Home with automatic TLS (if enabled)
$H
```

---

## 📖 How to use

1. Open the **RapidChangeATC** dialog from the Tools menu.
2. Select your **Collet Size** and **Model**.
3. Configure the number of **Pockets**, **Orientation**, and **Direction**.
4. Set the **Pocket 1** location using the **Grab** button.
5. Set the **Tool Setter** location using the **Grab** button.
6. Optionally configure the **Manual Tool** location.
7. Adjust RPM and the remaining settings as needed.
8. Save the configuration.

---

## ⚙️ Configuration options

### ATC settings
- **Collet Size** — ER11, ER16, ER20, ER25, ER32
- **Model** — Basic, Pro, Premium
- **Number of Pockets** — 1 to 8
- **Orientation** — X or Y axis
- **Direction** — positive or negative
- **Pocket Distance** — distance between pockets (mm)

### Position settings
- **Pocket 1** — X/Y location of the first pocket
- **Tool Setter** — X/Y location of the tool length setter
- **Manual Tool** — X/Y location for manual tool operations

### Tool change settings
- **Load RPM** — spindle speed for loading tools
- **Unload RPM** — spindle speed for unloading tools
- **Engage Feedrate** — feed rate for pocket engagement
- **Spindle At Speed** — wait for the spindle to reach speed
- **ATC Start Delay** — delay before starting the ATC sequence (0–10 s)

### Tool setter settings
- **Seek Distance** — probe travel distance (mm)
- **Seek Feedrate** — probe feed rate (mm/min)
- **Tool Sensor** — Probe/TLS or Aux port selection

### Premium features
- **Cover Open Command** — G-code to open the dust cover
- **Cover Close Command** — G-code to close the dust cover

### Probe tool (tool 99)
- **Add Probe** — enable probe tool support
- **Probe Load G-code** — custom G-code for loading the probe
- **Probe Unload G-code** — custom G-code for unloading the probe

### Advanced settings
- **Show Macro Commands** — display expanded G-code in the terminal
- **Perform TLS after HOME** — automatic TLS after the first homing

### Advanced settings (JSON only)

These are edited directly in the plugin settings JSON:

```json
{
  "zEngagement": -50,
  "zSafe": 0,
  "zSpinOff": 23,
  "zRetreat": 7,
  "zProbeStart": -20,
  "zone1": -27.0,
  "zone2": -22.0
}
```

---

## 🔧 Technical details

- `commands.js` is pure command-processing logic — no `import`/`require`/`fetch`/`ctx` — so it runs on Node.js natively and inside the .NET Jint sandbox (`pro-v2` runtime).
- `config.html` is the client-side configuration dialog; it reads and writes plugin settings through `/api/plugins/com.ncsender.rapidchangeatc/settings`.
- Hooks the `onBeforeCommand` event and requires the `gcode.modify` permission.
- Operator-facing dialog text lives in the `messages` block of `manifest.json`.

---

## 🚀 Releases

Releases are built by the `release-build.yml` GitHub Actions workflow, which fires on any pushed tag matching `v*.*.*` and refuses to build if the tag doesn't match the `version` field in `manifest.json`. Each release publishes both a versioned zip and a `-latest.zip` copy at a stable download URL.

To cut a release locally:

```bash
.scripts/bump-release.sh patch
```

`patch`, `minor`, `major`, or an explicit `X.Y.Z` all work. The script bumps `manifest.json`, opens `latest_release.md` for release notes, commits, tags, and pushes.

---

## 📄 License

GNU General Public License v3.0 — see [LICENSE](LICENSE).

Copyright (C) 2024 Francis Marasigan (original author)
Copyright (C) 2026 Compwiser1 (this fork)
