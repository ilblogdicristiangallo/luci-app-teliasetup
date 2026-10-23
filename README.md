# luci-app-teliasetup
LuCI app for ZTE MF286D that manages sequential FOTA firmware updates. Automatically detects the current version (B02-B12) and applies only the required updates. Bilingual web interface (IT/EN) with progress bar and live log.
# 🚀 luci-app-teliasetup

<p align="center">
  <img src="https://img.shields.io/badge/OpenWrt-25-blue?style=flat-square&logo=openwrt" alt="OpenWrt 25">
  <img src="https://img.shields.io/badge/LuCI-App-orange?style=flat-square" alt="LuCI App">
  <img src="https://img.shields.io/badge/Modem-ZTE%20MF286D-red?style=flat-square" alt="ZTE MF286D">
  <img src="https://img.shields.io/badge/Firmware-B02%20→%20B12-green?style=flat-square" alt="B02 to B12">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=flat-square" alt="MIT License">
  <img src="https://img.shields.io/badge/Language-IT%20%2F%20EN-blueviolet?style=flat-square" alt="IT/EN">
</p>

<p align="center">
  <b>LuCI app for OpenWrt 25 to automatically update ZTE MF286D modem firmware sequentially from B02 to B12.</b>
</p>

<p align="center">
  Features auto-detection, ADB transfer, AT commands, progress bar, live log, retry mechanism and bilingual support (IT/EN).
</p>

---

## 📸 Screenshots

<table>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/ilblogdicristiangallo/luci-app-teliasetup/main/ScreenUSE/luci-teliaupdate.png" alt="Telia FOTA Setup - Main View" width="100%">
      <br>
      <sub><b>Main interface</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/ilblogdicristiangallo/luci-app-teliasetup/main/ScreenUSE/luci-teliaupdate2.png" alt="Telia FOTA Setup - Progress" width="100%">
      <br>
      <sub><b>Progress bar and live log</b></sub>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="https://raw.githubusercontent.com/ilblogdicristiangallo/luci-app-teliasetup/main/ScreenUSE/luci-teliaupdate3.png" alt="Telia FOTA Setup - Completed" width="100%">
      <br>
      <sub><b>Update completed</b></sub>
    </td>
  </tr>
</table>

---

## 📋 Description

**Telia FOTA Setup** is a LuCI app for OpenWrt 25 that allows you to sequentially update the ZTE MF286D modem firmware from version **B02** up to **B12**, in an automatic and guided way.

The app detects the currently installed version on the modem and applies only the necessary updates, automatically handling:

- 📤 Delta file upload via ADB
- 📡 AT FOTA commands for flashing
- ✅ Version verification after each update
- 🔄 Retry on error (max 2 attempts)
- 💾 Version cache to avoid overloading the modem

---

## ✨ Features

- 🌍 **Bilingual interface**: Italian and English
- 🔍 **Auto-detection** of current firmware version (B02 → B12)
- 📊 **Real-time progress bar** with percentage
- 📝 **Live log** updated every 5 seconds
- 🔒 **Lock mechanism** to prevent double execution
- ⚙️ **Version cache** with 10-minute TTL
- 🕒 **Cron job** updates cache every 5 minutes
- 🛡️ **Retry mechanism** (max 2 attempts per update)
- 🎯 **Smart update path**: only required versions are installed
- 💾 **Automatic file management** in `/tmp/`

---

## ⏱️ Timing

Each update package goes through two phases, each with its own countdown:

| Phase | Countdown |
|-------|-----------|
| Flash in progress | 6 min 10 s (370 s) |
| Waiting for new FOTA | 15 s |
| **Total countdown per package** | **6 min 25 s (385 s)** |

On top of the countdowns there are about 70 s of fixed pauses (5 s after every ADB upload and every AT command) and about 10 s for the version check, so one package takes roughly **7 min 45 s**, plus the ADB transfer time.

The full path from B02/B03/B04 installs **7 packages** (B05, B06, B08, B09, B10, B11, B12), for a total countdown of **44 min 55 s** and about **54 min** overall. Version B07 has no dedicated package: from B06 or B07 the next step is B08.

| Starting version | Packages to install | Total countdown |
|------------------|---------------------|-----------------|
| B02 / B03 / B04 | 7 (B05 → B12) | 44 min 55 s |
| B05 | 6 (B06 → B12) | 38 min 30 s |
| B06 / B07 | 5 (B08 → B12) | 32 min 5 s |
| B08 | 4 (B09 → B12) | 25 min 40 s |
| B09 | 3 (B10 → B12) | 19 min 15 s |
| B10 | 2 (B11 → B12) | 12 min 50 s |
| B11 | 1 (B12) | 6 min 25 s |
| B12 | 0 (already up to date) | none |

---

## 🔧 Requirements

- **OpenWrt 25** (Compatibility on `ZTE MF286D`)
- **OpenWrt 24** (Compatibility on `ZTE MF286D`)
- **ZTE MF286D modem** with USB serial active
- **Free RAM**: at least 80 MB in `/tmp/`
- **Free flash**: at least 10 MB in `/overlay/`
- **Dependencies**: `luci-base`, `adb`

---

## 📦 Installation

### From APK package

1. Download the latest APK from [Releases](https://github.com/ilblogdicristiangallo/luci-app-teliasetup/releases)
2. Copy the APK to `/tmp/` on the router (using WinSCP, SCP, or similar)
3. Install:

<pre>apk add --allow-untrusted /tmp/luci-app-teliasetup-1.0.0-r1.apk</pre>
