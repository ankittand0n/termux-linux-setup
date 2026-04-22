# termux-linux-setup

Termux desktop bootstrap script for Android with support for XFCE4, LXQt, MATE, and KDE Plasma.

This fork adds stability improvements and optional features that users requested in upstream issues, including optional TigerVNC setup and cleanup/uninstall tooling.

## What This Fork Improves

- Fixes KDE launcher bug that could produce `nexec: command not found`
- Adds optional compatibility renderer mode (`llvmpipe`) to avoid `failed to load driver: zink`
- Removes conflicting `vulkan-loader-generic` when present
- Adds XKB setup defaults for Termux-X11 keyboard initialization issues
- Improves LXQt setup by installing `openbox` and creating a default LXQt WM config
- Adds toggles to skip optional app installation and desktop shortcut generation
- Adds generated `uninstall-linux.sh` cleanup helper
- Adds optional TigerVNC server setup with `start-vnc.sh` and `stop-vnc.sh`

## Prerequisites

Install these on Android first:

1. Termux (F-Droid build, not Google Play)
   - https://f-droid.org/en/packages/com.termux/
2. Termux-X11 app (nightly APK)
   - https://github.com/termux/termux-x11/releases/tag/nightly

## Install

From inside Termux:

```bash
pkg update -y && pkg upgrade -y
pkg install -y curl
curl -LO https://raw.githubusercontent.com/ankittand0n/termux-linux-setup/main/termux-linux-setup.sh
chmod +x termux-linux-setup.sh
./termux-linux-setup.sh
```

If running directly from a local clone:

```bash
chmod +x termux-linux-setup.sh
./termux-linux-setup.sh
```

## Interactive Options During Setup

The installer now asks whether to:

- Install optional GUI apps (Firefox, VLC, Git, Wget, Curl)
- Create desktop shortcuts
- Install and configure TigerVNC
- Enable or disable Zink acceleration

## Generated Commands

After install, these helper scripts are created in your home directory:

- `./start-linux.sh` - start Termux-X11 desktop session
- `./start-linux-safe.sh` - start with compatibility rendering mode (no Zink)
- `./stop-linux.sh` - stop desktop session
- `./uninstall-linux.sh` - cleanup generated files and optionally remove packages

If VNC was enabled:

- `./start-vnc.sh` - start VNC server on display `:1` (port `5901`)
- `./stop-vnc.sh` - stop VNC server on display `:1`

You can choose another VNC display by passing an argument, for example:

```bash
./start-vnc.sh 2
./stop-vnc.sh 2
```

## Troubleshooting

- Zink errors (`failed to load driver: zink`):
  - Run `./start-linux-safe.sh`
  - Re-run installer and disable Zink when prompted
- Keyboard/XKB errors in Termux-X11:
  - Ensure latest Termux-X11 nightly is installed
  - The script now installs `xkeyboard-config` and exports `XKB_CONFIG_ROOT`
- LXQt starts broken or asks for WM:
  - This fork installs `openbox` and writes LXQt session WM config automatically

## Notes

- KDE Plasma is resource-heavy and may not be stable on all devices.
- If your device is non-Adreno or unstable with Vulkan, compatibility mode is usually more reliable.
