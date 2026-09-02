# Using VoidOS

Everything you can press, type and install. Written for people using VoidOS, not
building it.

If you never open a terminal, the [Shortcuts](#keyboard-shortcuts) and
[Installing applications](#installing-applications) sections are the ones you
want.

---

## Contents

- [Installing applications](#installing-applications)
  - [The graphical way](#the-graphical-way)
  - [The reliable way](#the-reliable-way)
  - [Two things that surprise people](#two-things-that-surprise-people)
  - [Managing what you installed](#managing-what-you-installed)
  - [Where applications may read and write](#where-applications-may-read-and-write)
  - [Building from source](#building-from-source-advanced)
- [Keyboard shortcuts](#keyboard-shortcuts)
  - [Opening things](#opening-things)
  - [Windows](#windows)
  - [Screenshots](#screenshots)
  - [Hardware keys](#hardware-keys)
  - [Mouse](#mouse)
  - [In the terminal](#in-the-terminal)
- [Commands](#commands)
  - [Applications you can open](#applications-you-can-open)
  - [Screen and session](#screen-and-session)
  - [Sound, screen and input](#sound-screen-and-input)
  - [Appearance](#appearance)
  - [Updates and recovery](#updates-and-recovery)
  - [Disks and network](#disks-and-network)
  - [Security and troubleshooting](#security-and-troubleshooting)
- [Getting out of trouble](#getting-out-of-trouble)

---

## Installing applications

VoidOS installs applications with **Flatpak**. Flathub — the main Flatpak
catalogue — is already set up, so you do not have to add it. Nothing is
pre-installed; you choose what you want.

### The graphical way

**Settings → Apps → Get applications.**

Be aware of the limit: the graphical catalogue currently offers only **three**
applications (Signal, Element and Briar). There is no search box yet. For
anything else, use the terminal below. A proper Apps screen is planned.

### The reliable way

Open a terminal (**Super+T**) and install by name:

```sh
flatpak install flathub org.mozilla.firefox
```

**No `sudo`.** You will be asked for your password by a normal dialog the first
time. Then run it from the menu like any other application, or:

```sh
flatpak run org.mozilla.firefox
```

Application IDs look like `org.videolan.VLC` or `com.spotify.Client`. Find them
on [flathub.org](https://flathub.org) — search there, then install by ID.

### Two things that surprise people

**`flatpak search` finds nothing on a fresh install.** The catalogue index is not
downloaded until you ask for it. Fix it once:

```sh
flatpak update --appstream
```

Or press **Download** in Settings → Apps. Installing by exact ID works fine
without this — only *searching* needs it.

**The first install on Secure or Maximum can take several minutes.** Everything
goes through Tor, and the download waits for Tor to be ready before it starts. It
is not stuck. Later installs are quicker.

### Managing what you installed

```sh
flatpak list --app              # what you have
flatpak update                  # update everything
flatpak uninstall org.videolan.VLC
flatpak uninstall --unused      # reclaim space from leftovers
```

### Where applications may read and write

By default a Flatpak application can only see your **Downloads**, **Documents**
and **Pictures** folders. The rest of your home folder is not visible to it.

This is deliberate: an application you installed cannot quietly read everything
you own. If something genuinely needs another folder, you can grant it:

```sh
flatpak override --user --filesystem=~/Music org.videolan.VLC
```

Grant the narrowest thing that works. Granting `home` gives it everything.

### Building from source (advanced)

`voidos-build` compiles software from source. It is slower and needs a one-time
setup, but reaches things Flathub does not carry.

```sh
voidos-build setup-user          # once per account, takes a while
voidos-build search nmap
voidos-build install net/nmap
voidos-build list
```

It installs into your home folder by default. If you have no persistent storage
set up it will warn you first, because anything built would be lost at restart.

---

## Keyboard shortcuts

**Super** is the Windows key.

**None of these work while the screen is locked** — that is deliberate. Unlock
first.

### Opening things

| Press | Does |
|---|---|
| **Super** (tap alone) | Activities overlay — start typing to search, Enter opens the first result, Esc closes |
| **Super + D** | Same overlay, if tapping Super is awkward on your keyboard |
| **Super + T** | Terminal |
| **Super + E** | Files |
| **Super + K** | On-screen keyboard |
| **Super + L** | Lock the screen |

### Windows

| Press | Does |
|---|---|
| **Super + ↑** | Maximise / restore |
| **Super + ↓** | Minimise |
| **Super + ←** | Snap to left half |
| **Super + →** | Snap to right half |
| **Super + F** *or* **F11** | Full screen |
| **Super + Q** *or* **Alt + F4** | Close the window |
| **Alt + Tab** | Next window |
| **Alt + Shift + Tab** | Previous window |

### Screenshots

| Press | Does |
|---|---|
| **Print Screen** | Whole screen |
| **Shift + Print Screen** | Drag a rectangle to capture just that area |

Both save to `~/Pictures/Screenshots/` with the date and time as the filename.
There is no flash or confirmation — the file is simply there.

### Hardware keys

Volume up, down and mute; microphone mute; and brightness up and down all work as
labelled. Muting the microphone shows a notification, so it is never a silent
change you forget about.

**The power button** opens a menu — Lock, Suspend, Restart, Shut Down — so a
stray press cannot shut you down. **Holding it for about five seconds** forces
the machine off, which is your way out if something is frozen.

### Mouse

| Do | Gets |
|---|---|
| **Double-click** a titlebar | Maximise / restore |
| **Right-click** a titlebar | That window's menu |
| **Scroll** on a titlebar | Roll the window up into its titlebar, and back |
| **Drag** a titlebar or **Alt + drag** anywhere | Move the window |
| **Drag** an edge or corner | Resize |

### In the terminal

The terminal is kitty with its standard shortcuts. **Ctrl + Shift** plus:
**C** copy, **V** paste, **T** new tab, **Enter** split, **E** label every link
on screen so you can open one by keystroke, and **F6** to list every binding it
has.

---

## Commands

You never need these for normal use. They exist when you want them.

### Applications you can open

| Command | Opens |
|---|---|
| `void-settings` | Settings |
| `void-monitor` | System Monitor |
| `void-disks` | Disks |
| `void-firewall` | Firewall |
| `void-installer` | Install VoidOS to this machine |

### Screen and session

| Command | Does |
|---|---|
| `voidos-lock` | Lock the screen now. Clears the clipboard first, and reports loudly if the lock fails rather than pretending it worked |
| `void-shutdown-ui` | The power dialog the power button opens |
| `void-power shutdown` | Shut down. Also `reboot`, `suspend`, `hibernate`, `lockAll`, `suspendAndLock` — spelling and capitals matter |

### Sound, screen and input

| Command | Does |
|---|---|
| `voidos-mic toggle` | Mute/unmute the microphone. Also `on`, `off`, `status` |
| `voidos-backlight` | Print screen brightness. `set 60` sets it; `+10` / `-10` adjust; `kbd get` / `kbd set 50` for a lit keyboard; `devices` lists what it found |
| `voidos-osk toggle` | On-screen keyboard. Also `show`, `hide` |
| `voidos-keyboard` | Print your layout. `voidos-keyboard de` changes it; add a variant like `voidos-keyboard de nodeadkeys`. Refuses layouts it does not recognise, so it cannot leave you with a dead keyboard. Log out and back in to apply |

### Appearance

| Command | Does |
|---|---|
| `voidos-theme apply` | Re-apply your appearance settings if something looks wrong |
| `voidos-theme accent '#b79ced'` | Set the accent colour immediately |

### Updates and recovery

| Command | Does |
|---|---|
| `void-update status` | Which version you are on and what is on the spare slot |
| `void-update rollback` | Go back to the previous version at the next restart |

Use **Settings → Update** for normal updating — it is the same thing with a
progress bar.

### Disks and network

| Command | Does |
|---|---|
| `void-disk` | Disk helper used by the Disks application |
| `void-network` | Network helper used by Settings |
| `voidos-luks-backup` | Back up your disk-encryption header. Worth doing — if that header is damaged the disk is unrecoverable, even with the right passphrase |

### Security and troubleshooting

| Command | Does |
|---|---|
| `voidos-confine status` | How many applications are confined, and how |
| `voidos-confine check firefox` | Is this program confined, and by what |
| `voidos-confine denials` | What was recently blocked — the first thing to look at when an app misbehaves on Maximum |
| `voidos-build` | Install software from source (see above) |

Most `voidos-confine` subcommands need `sudo`.

---

## Getting out of trouble

**An application will not start on Maximum.** Run `sudo voidos-confine denials`
to see what was blocked. Switching to Secure in Settings is the quick answer.

**The screen looks wrong after changing settings.** `voidos-theme apply`.

**An update went badly.** **Settings → Update → Roll back**, then restart. The
previous version is still on the disk, so nothing is downloaded. If the machine
will not start at all, it goes back on its own.

**Something is frozen.** Hold the power button for about five seconds.

**You want to check your disk-encryption backup exists.** `voidos-luks-backup`.
Keep the result somewhere that is not the encrypted disk.
