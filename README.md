<!-- markdownlint-disable MD033 MD041 -->
<p align="center">
  <img
    src="assets/img/maintenancemanagerproplus.svg"
    alt="MaintenanceManagerProPlus logo"
    width="64"
    height="64"
  />
</p>
<h1 align="center">OctoPrint-MaintenanceManagerProPlus</h1>
<!-- markdownlint-enable MD033 MD041 -->

[![License][badge-license]](LICENSE)
[![Python][badge-python]](https://python.org)
[![OctoPrint][badge-octoprint]](https://octoprint.org)
[![Latest Release][badge-release]](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases/latest)
[![Pre-release][badge-prerelease]](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases)
[![Downloads][badge-downloads]](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases)
[![Made with Love][badge-love]](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus)

[badge-license]: https://img.shields.io/github/license/Ajimaru/OctoPrint-MaintenanceManagerProPlus?style=flat-square&logo=gnu&logoColor=white
[badge-python]: https://img.shields.io/badge/python-3.9%2B-blue.svg?style=flat-square&logo=python&logoColor=white
[badge-octoprint]: https://img.shields.io/badge/OctoPrint-1.x%20%7C%202.x-blue.svg?style=flat-square&logo=octoprint&logoColor=white
[badge-release]: https://img.shields.io/github/v/release/Ajimaru/OctoPrint-MaintenanceManagerProPlus?style=flat-square&logo=github&logoColor=white
[badge-prerelease]: https://img.shields.io/github/v/release/Ajimaru/OctoPrint-MaintenanceManagerProPlus?include_prereleases&label=prerelease&style=flat-square&logo=github&logoColor=white
[badge-downloads]: https://img.shields.io/github/downloads/Ajimaru/OctoPrint-MaintenanceManagerProPlus/total.svg?style=flat-square&logo=github&logoColor=white
[badge-love]: https://img.shields.io/badge/made_with-%E2%9D%A4%EF%B8%8F-ff69b4?style=flat-square

An OctoPrint plugin that works like an odometer for your printer: it adds up the print time
and the travel of every axis and the extruder, so you can plan maintenance such as
tightening belts or greasing rails.

## Disclaimer

> [!CAUTION]
> **About the codebase.** This is a personal project, in an early development stage,
> and I make no guarantees about its functionality or safety.
> It has not been fully tested and should not be used in production environments.
> **Use at your own risk.**

> [!NOTE]
> **About this project.** I maintain this fork for my own printer setup with AI,
> and if it helps others, even better.
> I have tested it to the best of my knowledge and ability.
> Disclosed here per the OctoPrint plugin guidelines. Issues and PRs are welcome.

## Origin

This plugin is a fork descending from:

- [OllisGit/OctoPrint-MaintenanceManager](https://github.com/OllisGit/OctoPrint-MaintenanceManager) — original project
- [viktak/OctoPrint-MaintenanceManager](https://github.com/viktak/OctoPrint-MaintenanceManager) — fork this repository is based on (MQTT support)

The original README is archived in [docs/original-readmes/](docs/original-readmes/).

This fork is maintained by [Ajimaru](https://github.com/Ajimaru).

## Requirements

- **OctoPrint 1.x or 2.x.** Last tested upstream with OctoPrint 1.7.2; a test on OctoPrint
  2.x is still pending. On 2.x, travel is counted for printers connected through OctoPrint's
  serial connector; other connectors are untested.
- **Python 3.9 or newer.**

## Installation

> [!WARNING]
> Create a backup of your OctoPrint instance before installing this plugin. If you already
> use the original MaintenanceManager plugin, read
> [Migrating from MaintenanceManager](#migrating-from-maintenancemanager) first.

This URL always installs the **latest version**, and updates an existing installation
in place:

```text
https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/archive/master.zip
```

### Via OctoPrint's Plugin Manager

*Settings* → *Plugin Manager* → *Get More…* → *… from URL*, paste the URL and
click *Install*, then restart OctoPrint.

### Via pip

Use the pip of the Python environment OctoPrint runs under, then restart OctoPrint:

```bash
pip install --upgrade https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/archive/master.zip
```

### Installing a specific version

All released versions are listed on the
[releases page](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases).

## Features

Counted from the plugin's first start, kept across restarts and never reset automatically:

- **Print time** — the total time of all prints, from start to end or cancel; pauses are not counted
- **Axis travel** — the distance moved on X, Y and Z, each axis counted on its own
  (a diagonal move adds to both X and Y)
- **Extruder travel** — the distance the extruder motor moved, retractions included,
  so this is not the filament used
- **Tracking start** — when counting began

The current values are shown in a popup in the lower right corner of OctoPrint's UI, and
can be published via MQTT (see [Optional plugins](#optional-plugins)).

Travel is calculated from the `G0`/`G1` commands OctoPrint sends to the printer. Moves from
OctoPrint's *Control* tab are counted too. Not counted:

- arc moves (`G2`/`G3`) — with arc fitting in the slicer, travel comes out too low
- homing, and moves started on the printer's own display
- prints running from the printer's SD card (their print time is counted)
- extruders other than the first

> [!NOTE]
> Maintenance reports, maintenance alerts and per-part tracking, planned in the original
> project, are not implemented yet.

## Optional plugins

- [MQTT](https://plugins.octoprint.org/plugins/mqtt/) — publishes the counters (seconds and
  millimetres) to a topic set in the plugin settings, by default
  `octoPrint/plugin/MaintenanceManagerProPlus`. Without it, the topic setting has no effect.

## Screenshots

![Tracking display](screenshots/trackingDisplay.png "The tracking popup")

## Migrating from MaintenanceManager

This plugin uses its own identifier, so OctoPrint treats it as a separate plugin: it starts
with empty counters and default settings, and nothing is taken over automatically.

The counters live in `data/<plugin identifier>/trackingValues.json` inside OctoPrint's base
folder (`~/.octoprint` on OctoPi). To keep them, stop OctoPrint, copy
`data/MaintenanceManager/trackingValues.json` to `data/MaintenanceManagerProPlus/`
(replacing the file there, if any) and start OctoPrint again. While OctoPrint is running,
the plugin overwrites that file every second.

Good to know:

- **Uninstall or disable the original MaintenanceManager** — with both active, both count
  and both show a popup.
- The default MQTT topic changed to `octoPrint/plugin/MaintenanceManagerProPlus`. Enter
  your old topic in the plugin settings, or update whatever reads it, such as Home Assistant.

## Code of Conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md).

## License

See [LICENSE](LICENSE).

`utils/odometer.py` is derived from OctoPrint's G-code interpreter, which is licensed under
the AGPLv3.
