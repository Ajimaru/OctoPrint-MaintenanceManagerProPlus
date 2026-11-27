# OctoPrint-MaintenanceManagerProPlus
[![Version](https://img.shields.io/badge/dynamic/json.svg?color=brightgreen&label=version&url=https://api.github.com/repos/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases&query=$[0].name)]()
[![Released](https://img.shields.io/badge/dynamic/json.svg?color=brightgreen&label=released&url=https://api.github.com/repos/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases&query=$[0].published_at)]()
![GitHub Releases (by Release)](https://img.shields.io/github/downloads/Ajimaru/OctoPrint-MaintenanceManagerProPlus/latest/total.svg)


OctoPrint-Plugin that tracks print-time (total & on parts), axis-movements and has the ability to create "Maintenance Reports".

It can also define "Maintenance Alerts", so that you never missed to tie up your belts ;-)

# WORKIN PROGRESS, NO FINAL RELEASE, yet!!!! Only a DEV-release

## Origin

This plugin is a fork descending from:

- [OllisGit/OctoPrint-MaintenanceManager](https://github.com/OllisGit/OctoPrint-MaintenanceManager) — original project
- [viktak/OctoPrint-MaintenanceManager](https://github.com/viktak/OctoPrint-MaintenanceManager) — fork this repository is based on (MQTT support)

## Tested with:
- OctoPrint 1.7.2:  with Python 3.7.3

## Features
- [x] Total XYZE linear movement (only G0, G1. No G2/G3 or G28 or other manuel movement via control box)
- [ ] Track Multi-Tool extrusion
- [x] Total print time (pause excluded)
- [ ] Track runtime/movement for predefined hardware parts (e.g. "#1 Nozzel 0.4", Duration=1d12h, Extrusion=123m )
- [ ] Track fan runtime (M106/M107)
- [ ] Add "Maintenance Item" (e.g. "Changed X-Belt", X-Movement was at: 5k 165m 16cm)
- [ ] List/Filter/Search for "Maintenance Report"
- [ ] Add multiple images to a "Maintenance Report"
- [ ] CSV Export of "Maintenance Report(s)"
- [ ] Define "Maintenance Alerts" on "total print time", "part print time", "axis movement"

## Screenshots

![trackingDisplay](screenshots/trackingDisplay.png "TrackingDisplay")

## Setup

There is no release of this fork yet. Once there is, install it via the bundled
[Plugin Manager](https://docs.octoprint.org/en/master/bundledplugins/pluginmanager.html) using this URL:

    https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases/latest/download/master.zip

The plugin identifier is now `MaintenanceManagerProPlus`. Uninstall the original MaintenanceManager first,
otherwise both plugins track and publish in parallel. Counters and settings of the original plugin are not
taken over.

## Versions

see [Release-Overview](https://github.com/Ajimaru/OctoPrint-MaintenanceManagerProPlus/releases/)

