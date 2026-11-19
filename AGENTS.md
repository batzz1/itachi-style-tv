# AGENTS.md — Itachi-style TV playbook

Rules for any AI agent operating on a TV with this repo.

## Safety
- Never uninstall system apps; only `pm disable-user --user 0`. Restore with `tools/restore.sh`.
- Back up first: package list, disabled list, getprop, settings.
- Ask before disabling anything the user might use (streaming apps, inputs, Bluetooth).
- Check which app owns the saved Wi-Fi (`dumpsys wifi | grep cname`) before disabling setupwraith; disabling it can delete the saved network.

## Device
- Android TV / Google TV only (Xiaomi, TCL, Hisense, Sony, OnePlus, Nvidia Shield, etc.).
- Not Samsung Tizen, LG webOS, Roku, VIDAA, Fire TV (Vega OS).

## Launcher
- Package: `com.gathin.tvhome`. Activity: `.MainActivity`.
- Dock apps: configured in `launcher/app/src/main/res/values/config.xml` (`dock_apps`).
- Header text color: black (changed from white). Pills: black. App tiles: white.
- RAM label: `R.id.ramLabel`, updated in `updateRam()`.
- Wallpaper: `/sdcard/Android/data/com.gathin.tvhome/files/wallpaper.jpg` (single image, 1920x1080, <300KB, under 4K pixels).

## Wake fix
- `screensaver_activate_on_sleep=0`. Optional: `stay_on_while_plugged_in=7`.

## Wi-Fi name
- Requires Location on + `ACCESS_FINE_LOCATION` granted. SSID read via `WifiManager.getConnectionInfo().getSSID()`.

## Build
- JDK 17 + Android SDK. `cd launcher && ./build-and-install.sh` with `TV_HOST` set.
- Or use prebuilt `dist/itachi-tv-home.apk`.

## Revert
- `tools/restore.sh` re-enables packages. Re-enable stock launcher: `pm enable --user 0 com.google.android.tvlauncher`.
- Remove launcher: `adb uninstall com.gathin.tvhome`.