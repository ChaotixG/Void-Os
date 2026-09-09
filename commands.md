# VoidOS commands

The commands you can type. You never need any of them for normal use — the
desktop and Settings cover everything — but they are here when you prefer the
keyboard.

This is a short, plain list. The system itself carries the complete one:

```sh
voidos help              # every command there is, grouped
voidos help <command>    # what one command does
<command> help           # the same, asked directly
```

`void` is a shorter name for `voidos`, so `void help` is the same thing. Every
command also answers `--help` and `-h`, and asking for help never changes
anything. For the full detail of any command below — its options, and what it
reads and writes — run `<command> help`.

---

## Everyday

| Command | What it does |
|---|---|
| `void-settings` | Open Settings |
| `void-monitor` | See what is running, and what is using memory and battery |
| `void-disks` | Manage drives and partitions |
| `void-power` | Lock, suspend, restart or shut down |
| `voidos-lock` | Lock the screen now |
| `voidos-theme` | Re-apply your appearance settings if something looks wrong |
| `voidos-keyboard` | Set the keyboard layout |
| `voidos-backlight` | Screen and keyboard brightness |
| `voidos-osk` | Show or hide the on-screen keyboard |
| `voidos-mic` | Mute, unmute or check the microphone |
| `void-clipboard` | Clipboard history |
| `void-session-mgr` | Save and restore your running session |
| `voidos-portal` | Sign in to a Wi-Fi captive portal |

## Privacy and security

| Command | What it does |
|---|---|
| `void-security-profile` | Show or switch the privacy level (Daily, Secure, Maximum) |
| `void-firewall` | Show or switch how the network is filtered |
| `void-network` | Switch the network mode |
| `voidos-confine` | Check which applications are confined, and troubleshoot one that misbehaves |
| `void-user-perms` | Per-application camera, microphone and device permissions |
| `voidos-inuse` | Check whether the camera or microphone is in use right now |
| `voidos-usb-policy` | Set how new USB devices are handled |
| `voidos-luks-backup` | Back up and restore your disk-encryption header |
| `void-panic` | Immediately lock the screen and power off |

## System and updates

| Command | What it does |
|---|---|
| `void-update` | Check for, apply, verify, roll back and repair the system |
| `void-installer` | Install VoidOS onto another drive |
| `void-disk` | Drive maintenance |
| `void-psi` | Live memory, CPU and I/O pressure readings |
| `voidos-scratch` | Show the overflow swap status |

## Tools and building

| Command | What it does |
|---|---|
| `void-get` | Install extra command-line tools, verified against the signing key |
| `voidos-build` | Build and install software from source |

## Getting help

| Command | What it does |
|---|---|
| `voidos help` | List every VoidOS command, grouped by what it is for |
| `voidos help <command>` | The help for one command, wherever it lives |
| `void help` | The same list — `void` is a shorter name for `voidos` |
| `voidos version` | The version you are running |

---

When a command needs an argument it does not have, or is given one it cannot
use, it tells you exactly what was wrong. The full help appears only when the
command name itself is not one it knows.

The commands VoidOS runs for itself — the desktop, the background services and
the small helpers other parts start — are deliberately left off this list. They
are not meant to be typed, and `voidos help` marks them as such on the machine.
