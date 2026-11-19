# Prompt: Set up an Itachi-style Android TV

Paste this into any AI coding agent (Claude Code, OpenCode, Cursor, etc.) that has shell access,
on a computer on the same Wi-Fi as the TV.

---

I want to set up my Android TV with the Itachi-style launcher from this repo (itachi-style-tv).
My TV model is: <MODEL NAME HERE>. Work on the real device, step by step, verify with screenshots
pulled from the TV, and ask me before anything that depends on my preferences.

This repo contains everything you need: `tools/` (debloat, theme, restore, tv.py), `launcher/`
(source), `dist/itachi-tv-home.apk` (prebuilt), `wallpapers/itachi-moon.jpg`, and `examples/`
(device-specific debloat lists). Reuse them; don't rewrite them.

## Connect
- Find the TV on my network (hostname, ARP, or ask for IP) and connect over ADB on port 5555.
- If port 5555 is closed, try Wireless debugging (Google TV) or scan for the ADB port.
- Tell me when the "Allow USB debugging?" prompt appears so I can accept it.

## Back up first
- Save the package list, disabled packages, getprop output and settings to a backup folder.
- Write a restore script.

## Debloat
- Pick the matching list from `examples/` (or the closest model). Apply with `TV_HOST=<ip> tools/disable.sh <list>`.
- Never uninstall; only disable. Ask me before disabling anything personal.

## Install the launcher
- Option A (fast): `adb install -r dist/itachi-tv-home.apk`, then
  `adb shell cmd package set-home-activity --user 0 com.gathin.tvhome/.MainActivity` and
  `adb shell pm disable-user --user 0 com.google.android.tvlauncher`.
- Option B (build): `cd launcher && ./build-and-install.sh` with TV_HOST set.

## Location (for Wi-Fi name)
- `adb shell settings put secure location_providers_allowed +gps`
- `adb shell pm grant com.gathin.tvhome android.permission.ACCESS_FINE_LOCATION`

## Wallpaper
- Push `wallpapers/itachi-moon.jpg` to `/sdcard/Android/data/com.gathin.tvhome/files/wallpaper.jpg`.

## Wake fix
- `adb shell settings put secure screensaver_activate_on_sleep 0`

## Verify
- Pull a screenshot (`adb exec-out screencap -p > screen.png`) and confirm: black header text,
  RAM label under the clock, Wi-Fi pill shows the network name, 4 dock apps.

## Close up
- Switch off USB debugging / network ADB, revoke authorizations, reboot, verify port 5555 is closed.