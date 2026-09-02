# VoidOS

A private, secure Linux desktop that stays out of your way.

VoidOS gives you a clean, modern desktop with real privacy protection built in —
including a one-click switch that sends everything you do through the Tor
network. You choose how private you want to be, and you can change your mind at
any time without reinstalling.

**Latest version: 0.9.3.7** —
[Download](https://github.com/ChaotixG/Void-Os/releases/latest)

---

## Contents

- [Who it's for](#who-its-for)
- [What's included](#whats-included)
- [Privacy levels](#privacy-levels)
  - [Daily](#daily)
  - [Secure](#secure)
  - [Maximum](#maximum)
  - [Switching between them](#switching-between-them)
- [Getting VoidOS](#getting-voidos)
  - [1. Download](#1-download)
  - [2. Check the download](#2-check-the-download)
  - [3. Put it on a USB stick](#3-put-it-on-a-usb-stick)
  - [4. Try it, then install it](#4-try-it-then-install-it)
- [Updating](#updating)
- [If an update goes wrong](#if-an-update-goes-wrong)
- [Everyday use](#everyday-use)
- [Useful commands](#useful-commands)
- [Full guide: commands, shortcuts and installing apps](USING.md)
- [Making it yours](#making-it-yours)
- [Known issues](#known-issues)
- [What VoidOS does not promise](#what-voidos-does-not-promise)
- [Coming soon](#coming-soon)
- [Planned releases](#planned-releases)
- [Getting help](#getting-help)
- [Licence](#licence)

---

## Who it's for

VoidOS is for anyone who wants a normal, comfortable computer that doesn't leak
their life. You do not need to be a Linux expert. If you can install an app and
follow four steps with a USB stick, you can run VoidOS.

It is a good fit if you want to browse without being tracked, keep your files
encrypted, or work on something sensitive. It is also fine as an everyday
machine — you can watch video, play music, write documents and browse the web.

---

## What's included

Everything below is installed and ready when you first boot. Nothing to set up.

| | |
|---|---|
| **Web browser** | LibreWolf — a privacy-focused browser with tracking protection on by default |
| **Password manager** | KeePassXC — your passwords stay on your machine |
| **Files** | Browse, copy and organise your files |
| **Documents** | Open and read PDFs and documents |
| **Media** | Play video and music; view images |
| **Text editing** | A simple editor, plus a full one if you prefer |
| **Archives** | Open and create zip files and archives |
| **Calculator** | Including unit conversion |
| **Terminal** | For when you want it — you never have to use it |
| **Settings** | Wi-Fi, appearance, users, privacy level, updates, everything |
| **Disks** | Format, mount and manage drives |
| **System Monitor** | See what's running and what's using your battery |
| **Firewall** | Control what can reach the network |

Printing and scanning, Bluetooth, and smartcards are supported.

---

## Privacy levels

This is the heart of VoidOS. There are three levels, and you pick one. Each is a
genuine trade-off between how private you are and how convenient things feel.

You can find them in **Settings → Security profile**.

### Daily

**Normal networking. The most convenient.**

Your internet connection works the way it does on any other computer. Websites
and services see your real connection, exactly as they would on Windows or a Mac.

Use this when you want everything to just work — video calls, games, streaming,
banking apps, anything that gets unhappy about unusual connections. Applications
are still confined and your disk is still encrypted; it is the *networking* that
is ordinary.

### Secure

**Everything goes through Tor.**

Your internet traffic is routed through the Tor network, which hides your real
location and makes it very hard to link what you do back to you. This happens for
the whole system, not just the browser — so anything on your machine that talks
to the internet is covered.

Expect pages to load more slowly, and expect some sites to challenge you or block
you outright, because they cannot tell who you are. That is Tor working, not
VoidOS failing.

Use this for day-to-day private work.

### Maximum

**Everything through Tor, and the system locks down.**

Everything in Secure, plus the strictest protection VoidOS offers. Applications
are held to their permissions rather than merely warned, and anything wanting
access to hardware like your camera or microphone has to ask you first, every
time.

Some applications will not work properly here, and that is deliberate — the
restrictions are not loosened to accommodate them. Choose Maximum when not being
identified matters more than convenience.

### Switching between them

Change level in **Settings → Security profile**. You do not need to reinstall,
and your files, applications and settings are untouched.

A note worth reading: **a VPN is not Tor.** In Secure and Maximum your traffic
already goes through Tor and a VPN does not add to it. If you want a VPN, use it
in Daily.

---

## Getting VoidOS

### 1. Download

Get the `.iso` file and the matching `.sha256` file from the
[latest release](https://github.com/ChaotixG/Void-Os/releases/latest).

### 2. Check the download

**Do this before you install.** It confirms the file arrived complete and was not
damaged or altered on the way.

Put the `.iso` and the `.sha256` in the same folder, then run the command for
your system. **You never have to compare the long code yourself** — each one
checks it for you and just tells you whether it passed.

**Linux**

```sh
sha256sum -c voidOS-x86_64-lean-0.9.3.7.iso.sha256
```

**macOS**

```sh
shasum -a 256 -c voidOS-x86_64-lean-0.9.3.7.iso.sha256
```

**Windows** — open PowerShell in that folder and paste both lines:

```powershell
$f = "voidOS-x86_64-lean-0.9.3.7.iso"
if ((Get-FileHash $f -Algorithm SHA256).Hash -eq (Get-Content "$f.sha256").Split(" ")[0]) { "OK" } else { "FAILED" }
```

You want **OK**. Anything else — *FAILED*, or no output at all — means the file
is not right: delete it, download it again, and check it again before installing.

*Why bother:* a half-finished download usually looks perfectly normal, and you
would not find out until the install failed partway through.

### 3. Put it on a USB stick

Use any USB stick of 4 GB or more. Writing to it erases everything on it.

- **Easiest:** [Ventoy](https://www.ventoy.net) — set it up once, then simply
  copy `.iso` files onto the stick like ordinary files.
- **Also fine:** balenaEtcher, Rufus, or `dd` if you know it.

### 4. Try it, then install it

Boot your computer from the USB stick. Most machines offer a boot menu if you
press **F12**, **F2**, **Esc** or **Del** just after switching on.

VoidOS starts as a live session, so you can look around and check your Wi-Fi and
screen work **before** changing anything on your computer. When you are happy,
open the installer from the desktop and follow it through.

The installer can encrypt your disk. Take that option unless you have a reason
not to, and choose a passphrase you will not forget — **it cannot be recovered.**

**Then do one more thing.** Once you have booted the installed system, back up
your encryption header to a USB stick. The header is a few megabytes at the start
of the disk that your passphrase unlocks; if it is ever damaged, the disk is
unrecoverable and the correct passphrase cannot help. It takes seconds, and it is
the one backup that cannot be made after the fact —
[how to do it](USING.md#disk-encryption-backup-and-recovery).

VoidOS runs on 64-bit PCs, and boots on both modern (UEFI) and older (BIOS)
machines.

---

## Updating

**Settings → Update → Check now → Install**, then restart.

Updates are safe by design. A new version is written to a *spare copy* of the
system while you carry on working, and only becomes active when you restart.

**Your files, settings and installed applications are never part of an update.**
They sit outside the system copies and are left exactly as they are.

---

## If an update goes wrong

You have two safety nets.

1. **It fixes itself.** If a new version fails to start, your computer notices and
   goes back to the previous one on its own. You do not have to do anything.
2. **You change your mind.** Go to **Settings → Update → Roll back**. The
   previous version is still on the disk, so nothing is downloaded and it only
   takes a restart.

---

## Everyday use

**Locking the screen.** Press **Super + L** (the Windows key and L), or use the
power menu in the bar at the bottom. Your password unlocks it. The screen also
locks itself after a while, and whenever the computer sleeps.

**The power button** opens a menu — Lock, Suspend, Restart or Shut Down — so a
stray press cannot shut you down and lose your work. Holding it for about five
seconds still forces the machine off if something is stuck.

**The bar at the bottom** appears when you move the mouse to the bottom of the
screen. It holds your apps, and on the right, the network, sound, battery and
power controls.

**Your files** live in your home folder — Documents, Downloads, Pictures and so
on, as you would expect.

---

## Useful commands

You never need the terminal for normal use. These exist if you like it.

**→ [USING.md](USING.md) is the full guide** — every command with its
subcommands, all 26 keyboard shortcuts, and how to install applications.

| Command | What it does |
|---|---|
| `void-settings` | Open Settings |
| `void-disks` | Manage drives and partitions |
| `void-monitor` | See what is running |
| `void-firewall` | Network permissions |
| `void-update` | Check for and install updates |
| `void-installer` | Install VoidOS to this computer |
| `voidos-lock` | Lock the screen now |
| `voidos-theme` | Re-apply your appearance settings |
| `voidos-keyboard` | Keyboard layout |
| `voidos-mic` | Mute or unmute the microphone |
| `voidos-backlight` | Screen brightness |
| `voidos-osk` | On-screen keyboard |

---

## Making it yours

**Appearance.** **Settings → Personalisation** — accent colour, wallpaper, dark
styling, text size, and animations.

**Your own settings and configuration** live in your home folder under
`~/.config`, in the normal Linux places. Editing them is entirely your business.

**Changing VoidOS itself.** You are free to modify VoidOS on any computer you
own, as much as you like. What you may not do is hand out a *changed* version to
other people, or call a changed version "VoidOS" — see [LICENCE](LICENSE).

**The open-source parts.** VoidOS is built on a large amount of open-source
software, and you are entitled to the source code for those parts. See
[SOURCE.md](SOURCE.md) for how to get it.

---

## Known issues

VoidOS is pre-1.0 and these are the things we know are wrong right now. They are
listed so you can work around them instead of discovering them the hard way.

### Always eject USB sticks before unplugging

**What happens:** copy a file to a USB stick, the progress bar finishes, you pull
the stick out — and the file on it is incomplete or missing.

**Why it matters:** the progress bar finishing does not mean the data has
actually reached the stick. This is true of Linux generally, not only VoidOS.

**What to do:** always use **Eject** or **Safely Remove** in Files, and wait for
it to say it is safe. If you are copying something large, give it a moment.
Treat this as mandatory, not optional.

### Steam and gaming do not work yet

**What happens:** Steam fails to start.

**What to do:** nothing yet — this is being worked on and gaming is a priority
for the project, not an afterthought. If gaming is your main use, VoidOS is not
ready for you today.

### No way to set up a Tor bridge from Settings

**What happens:** if your network or country blocks Tor outright, the Secure and
Maximum levels will fail to connect, and there is currently no screen for
entering a bridge to get around that.

**What to do:** use the Daily level on networks that block Tor. A proper Tor
settings screen — bridges, transports, connection status — is planned.

### The lid-close setting does nothing

**What happens:** Settings offers a choice for what closing the lid should do,
but the choice is ignored. Closing the lid always suspends.

**What to do:** nothing is at risk here — closing the lid still suspends and
still locks. Just be aware the setting has no effect yet.

### Edited system files can survive an update

**What happens:** if you hand-edit a file belonging to the system, your version
of that file keeps being used even after an update changes it. The update
installs correctly; your edit simply keeps winning.

**What to do:** prefer Settings and your own files in your home folder for
customisation. If you do edit system files and something behaves oddly after an
update, undo your edit first before reporting a bug. Detection for this is
planned.

### The firewall cannot be managed per application

**What happens:** the Firewall screen can switch how traffic is routed, but there
is no way to say "let this application reach the network, block that one".

**What to do:** the protection that matters most — the privacy level you choose —
works fully. Per-application control is planned.

### There is no app *browser* yet

**What happens:** you can install anything, but you cannot browse for it here.
The graphical catalogue in Settings → Apps offers only three applications and has
no search box.

**What to do:** find what you want on [flathub.org](https://flathub.org), then
install it by name — `flatpak install flathub <id>`. Full instructions in
[USING.md](USING.md). A proper Apps screen is planned.

### Some hardware is not supported

**What happens:** on a few laptop models — particularly some 2-in-1 convertibles
— the built-in camera does not work. Certain fingerprint readers are also
unsupported.

**Why:** these depend on drivers that do not exist for Linux at all, from any
distribution. It is not something VoidOS can fix.

**What to do:** a USB webcam works normally. Check the live session before
installing: if the hardware works there, it will work once installed.

### Some applications will not run at Maximum

**What happens:** applications that expect broad access to your system may fail
or misbehave at the Maximum level.

**What to do:** this is deliberate — the restrictions are not loosened to
accommodate an application. Use Secure or Daily if you need that application.

---

## What VoidOS does not promise

Please read this part.

VoidOS makes you **much harder to track**. It does not make you invisible, and no
operating system can.

- **Tor hides where you are connecting from. It does not hide who you tell.** Log
  into an account with your real name and you have identified yourself, whatever
  the network is doing.
- **How you behave can identify you** — the way you write, the times you are
  online, the accounts you use together.
- **Encryption protects a switched-off computer.** Once you have unlocked and
  logged in, your files are open. Shut down rather than suspend if you are
  worried about the machine being taken.
- **VoidOS is still pre-1.0.** It is stable enough for daily use, but things do
  change between versions, and there will be bugs.

**If being identified would put you in real danger, do not rely on this — or on
any single tool — by itself.**

---

## Coming soon

The next release focuses on repair and recovery:

- **Repair** — check the system against the official release and put right
  anything that is damaged, without touching your files, settings or installed
  applications.
- **Reinstall** — return the system side of your machine to factory condition
  while keeping everything of yours. Useful if you have broken something, or want
  to be certain nothing unwanted is left behind. It asks for confirmation first
  and tells you exactly what you will lose.

---

## Planned releases

Rough order, not fixed dates. Things move when something turns out to matter more.

| Version | What it brings |
|---|---|
| **Next** | Repair and reinstall — recover a damaged system without losing your data |
| **0.9.3.15** | Tor connection settings — bridges and transports, so Tor works on networks that block it |
| **0.9.4** | Housekeeping and cleanup |
| **0.9.10** | Apps — browse, install and manage applications, in one place |
| **0.9.20** | Firewall — decide what each application is allowed to reach |
| **0.9.30** | Window management — alt-tab, workspaces, and snapping windows to screen edges |
| **0.9.50** | Performance — boot time, memory use and responsiveness, measured before and after |

Also planned, not yet scheduled:

- **Gaming** — getting Steam working; a main use case for the project
- **Installing tools during a session** — add what you need without rebuilding
  or reinstalling
- **Making the lid-close setting actually work**

Progress happens in the open — see
[Releases](https://github.com/ChaotixG/Void-Os/releases) for what has actually
shipped.

---

## Getting help

Found a bug, or something behaved unexpectedly?
[Open an issue](https://github.com/ChaotixG/Void-Os/issues) and say what you
expected and what happened instead. Please include which version you are running
(**Settings → About**) and which privacy level you were in.

---

## Licence

VoidOS is free to download and use, on as many of your own computers as you like.
You can modify it for yourself. You can pass on the official image unchanged. You
cannot hand out a modified version or use the VoidOS name for one.

Full terms: [LICENCE](LICENSE) · Open-source components:
[SOURCE.md](SOURCE.md)

VoidOS is an independent project. It is not affiliated with, derived from, or
endorsed by Void Linux.
