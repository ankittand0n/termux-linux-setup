# termux-linux-setup

Termux setup script for Android with two install modes:

- Native Termux desktop mode (XFCE4, LXQt, MATE, KDE Plasma)
- Linux distro container mode via proot-distro (Debian, Ubuntu, Arch Linux)

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
- Adds install mode selection: Native desktop or proot distro
- Adds distro selection support: Debian, Ubuntu, Arch Linux

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

The installer first asks for install mode:

- Native Termux Desktop
- Linux Distro (proot): Debian, Ubuntu, or Arch Linux

If you choose native desktop mode, it then asks whether to:

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

If distro mode was selected:

- `./start-distro.sh` - login to selected distro container
- `./update-distro.sh` - update distro packages
- `./remove-distro.sh` - remove selected distro container

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
- Distro mode install fails:
  - Run `proot-distro install debian` (or `ubuntu` / `archlinux`) manually to inspect errors

## Notes

- KDE Plasma is resource-heavy and may not be stable on all devices.
- If your device is non-Adreno or unstable with Vulkan, compatibility mode is usually more reliable.
- Distro mode installs a CLI container by default; desktop/VNC inside the distro is optional and can be installed later.
