# VoidOS

A private, secure Linux desktop that stays out of your way.

VoidOS gives you a clean, modern desktop with real privacy protection built in —
including a one-click switch that sends everything you do through the Tor
network. You choose how private you want to be, and you can change your mind at
any time without reinstalling.

**Latest version: 0.9.3.11** —
[Download](https://github.com/ChaotixG/Void-Os/releases/latest)

---

## Contents

- [Who it's for](#who-its-for)
- [New in 0.9.3.11](#new-in-09311)
- [New in 0.9.3.10](#new-in-09310)
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
- [Reinstalling without losing your files](#reinstalling-without-losing-your-files)
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

## New in 0.9.3.11

A small release, about a live stick keeping what you tell it.

- **Your settings survive a reboot on a stick with a persistence volume.** They
  used to be kept in memory and quietly return to their defaults every time. The
  tool channel, whether updates are checked automatically, the firewall's network
  mode, the confinement mode, hardware-address randomisation, power mode, charge
  limit, lid action, screen brightness, keyboard layout and per-app device
  permissions are all now written to the volume on *Daily* and *Secure*. Your
  privacy level is deliberately **not** among them: on a stick the boot menu
  entry you pick decides it, every time. **Settings → Storage & persistence**
  says which of the two states this session is in rather than leaving you to find
  out at the next boot. [Settings on a live stick](USING.md#settings-on-a-live-stick)
- **A password you change is saved within seconds.** Changing it with `passwd` —
  which is what **Settings → Accounts → Change password** runs — used to reach the
  volume only at a clean shutdown, so a stick that lost power came back with the
  old password. It is now recorded a few seconds after the change.
  [Changing your password](USING.md#changing-your-password)
- **Making someone an administrator now takes effect everywhere, and a stick
  cannot lock you out.** Granting or revoking administrator in **Settings →
  Accounts** also updates which account is asked for when a privacy level is
  changed. Revoking the only administrator is refused. A stick with a persistence
  volume now remembers all of your accounts rather than only the first, so
  promoting a second account and demoting the first survives a reboot — and if a
  machine ever had no administrator, VoidOS gives it back to the account you made
  first instead of leaving a stick that can authorise nothing.
  [Administrators](USING.md#administrators)
- **Building tools from source is smoother.** `voidos-build` now sets itself up
  on your first build instead of needing a separate step, you can send builds to
  another folder with `VOIDOS_BUILD_PREFIX`, and after a build it tells you the
  command you actually run (installing `netcat` gives you `nc`, and it says so).
  `voidos-build search` searches names as well as descriptions and says where to
  look when nothing matches. The full pkgsrc collection is buildable — over 700
  security packages among them — and the wireless-auditing tools ship ready to
  run. [Building from source](USING.md#building-from-source-advanced)
- **Every command explains itself, and says plainly when it fails.** `voidos
  help` lists them all and `<command> help` explains one; a command given a bad
  argument now tells you what was wrong instead of printing its whole help. There
  is a plain command reference in [commands.md](commands.md).
- **Clearer words from the updater.** The update log now names the temporary
  copies it clears from `/boot` instead of removing them silently, and rolling
  back an update that was never started no longer reports a pending update that
  cannot happen.
  [Updates and recovery](USING.md#updates-and-recovery)

**Updating from 0.9.3.10: there is nothing to do.** It is an ordinary update —
staged on the disk, not in memory, and it carries its own kernel and startup
image — so it installs and restarts like any other. **Settings → Update → Check
now → Install**.

---

## New in 0.9.3.10

The previous release, and all still true. Each of these has a fuller
explanation in [USING.md](USING.md).

- **Command-line tools, installed into the machine you are already using.**
  `void-get` fetches a tool from a signed list and puts it in place — no
  rebuild, no reinstall. A tool is either **ready-made** (downloaded and placed,
  seconds) or **built here** (compiled through pkgsrc, minutes to hours); the
  list says which, and you do not choose. Where it lands is your privacy level's
  decision: Daily keeps it, Secure asks you each time, and Maximum installs it
  into memory for the session only. Also in **Settings → Apps → Get tools**.
  [Getting tools](USING.md#getting-tools)
- **Every command explains itself.** `voidos help` lists every VoidOS command
  there is, grouped by what it is for. `voidos help <command>` — or
  `<command> help` — gives one command's own help: what it does, every option,
  every file it touches, and what each exit code means. `void` is a shorter name
  for the same thing. Asking for help never does anything.
  [Getting help on the command line](USING.md#getting-help-on-the-command-line)
- **One USB stick is enough.** A persistence volume can now go in the stick's
  own free space, after the image, without disturbing it. The stick still boots
  on both BIOS and UEFI machines and the image is untouched.
  [How big a USB stick](USING.md#how-big-a-usb-stick)
- **Your account is remembered on a stick.** Set up an account once on a stick
  with persistence and later boots go straight to the login screen. Setup runs
  once, not every time. On *Maximum* nothing is restored, by design.
  [How big a USB stick](USING.md#how-big-a-usb-stick)
- **Maximum forgets on an installed disk too.** An installed machine set to
  *Maximum* boots amnesic — no accounts, files, keyring or saved networks in the
  session — with only the tools you installed brought across, read-only. The
  live menu gains a matching entry, **Maximum with Persistence (tools only)**.
  [What a Maximum session carries](USING.md#what-a-maximum-session-carries)
- **Encrypted scratch space, and memory pressure handled before something
  dies.** Where there is persistent storage, overflow memory goes to a disk area
  encrypted with a random key that only ever exists in RAM. Separately,
  `void-psi` throttles a runaway application, then asks it to close, then forces
  it — telling you which and why, and never touching the desktop, the system
  services or Tor. [Memory and scratch space](USING.md#memory-and-scratch-space)
- **Repair.** **Settings → Update → Recovery** now says what is actually wrong
  with the system, one line per check, and puts back what it can from the copy
  of VoidOS you are running. [Repair](USING.md#repair)
- **Kernel updates that keep their slot, and put themselves right.** From the
  first update this version makes, each system copy keeps its own kernel and
  startup image, named for the version it holds and stored with a signature, so
  an update cannot overwrite the other copy's kernel and going back always finds
  the one it was built with. A machine that arrived here from 0.9.3.9 boots once
  on the old startup image, and the step that confirms a good boot brings it up
  to date by itself — nothing to run, nothing to repair by hand.
  [The first boot after upgrading from 0.9.3.9](USING.md#the-first-boot-after-upgrading-from-0939)
- **Drive and stick sizes are guidance, not a limit.** The installer refuses
  only a drive the system physically will not fit on, says exactly how much
  space that would take, and otherwise tells you what a small drive will cost
  before it touches anything.
  [How big a drive to install onto](USING.md#how-big-a-drive-to-install-onto)
- **Updates no longer depend on how much memory you have.** An installed system
  now stages the download on the persistent volume instead of in memory. If you
  are coming from 0.9.3.9 on a machine with 3 GB of RAM or less, that older
  updater will refuse this update with *"not enough space to stage the update"*
  and change nothing; install it by booting the 0.9.3.10 stick and choosing the
  reinstall that keeps your files.
  [Updating from 0.9.3.9 on a machine with 3 GB of RAM](USING.md#updating-from-0939-on-a-machine-with-3-gb-of-ram)
- **Installing and setting up leave a record.** The installer writes
  `/run/voidos/install.log` and creating a persistence volume writes
  `/run/voidos/persist-create.log` — both readable by administrators only, both
  in memory and gone at shutdown, and neither ever contains a passphrase.
  [Security and troubleshooting](USING.md#security-and-troubleshooting)

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

**Normal networking. The most convenient. This is the default.**

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
sha256sum -c voidOS-x86_64-lean-0.9.3.11.iso.sha256
```

**macOS**

```sh
shasum -a 256 -c voidOS-x86_64-lean-0.9.3.11.iso.sha256
```

**Windows** — open PowerShell in that folder and paste both lines:

```powershell
$f = "voidOS-x86_64-lean-0.9.3.11.iso"
if ((Get-FileHash $f -Algorithm SHA256).Hash -eq (Get-Content "$f.sha256").Split(" ")[0]) { "OK" } else { "FAILED" }
```

You want **OK**. Anything else — *FAILED*, or no output at all — means the file
is not right: delete it, download it again, and check it again before installing.

*Why bother:* a half-finished download usually looks perfectly normal, and you
would not find out until the install failed partway through.

### 3. Put it on a USB stick

**4 GB** or more runs VoidOS from the stick. To also keep a **persistence
volume** — your files, your settings, your account and the tools and
applications you install — use **6 GB at the very least, and 8 GB or more for
room to work in**. The volume goes in the stick's own free space, after the
image, so one stick does both. Writing the image to the stick erases everything
on it.

A stick is not updated in place: you update it by writing the new image to it,
exactly as you wrote the first one, and **your persistence volume is kept** —
it is a separate partition and the new system finds it as before. See
[How big a USB stick](USING.md#how-big-a-usb-stick).

- **Easiest:** [Ventoy](https://www.ventoy.net) — set it up once, then simply
  copy `.iso` files onto the stick like ordinary files.
- **Also fine:** balenaEtcher, Rufus, or `dd` if you know it.

### 4. Try it, then install it

Boot your computer from the USB stick. Most machines offer a boot menu if you
press **F12**, **F2**, **Esc** or **Del** just after switching on.

VoidOS starts as a live session, so you can look around and check your Wi-Fi and
screen work **before** changing anything on your computer. When you are happy,
open the installer from the desktop and follow it through.

The installer asks how you want your disk protected, and offers three choices.

- **Passphrase** — recommended, and what the installer picks for you. Your disk
  is encrypted, and you type a passphrase every time the computer starts. Choose
  one you will not forget — **it cannot be recovered.**
- **Automatic unlock** — your disk is still encrypted, but a key kept on the
  computer unlocks it for you, so there is no passphrase to type at start-up. Be
  clear about what that is worth. It does **not** protect you if someone takes
  the whole computer, because the key goes with it. What it does do is keep your
  data unreadable if the storage is taken out and read on another machine.
- **No encryption** — nothing on the disk is encrypted. Anyone who can read the
  drive can read your files, including someone who simply takes the machine.

This choice is about the disk only. It is independent of the privacy level:
Secure and Maximum behave exactly the same on an unencrypted disk — what they
protect is what the running system does, and what encryption protects is what
sits on the disk when it is off. Pick each on its own merits.

**If your disk is encrypted — either of the first two choices — then do one
more thing.** Once you have booted the installed system, back up your encryption
header to a USB stick. The header is a few megabytes at the start of the disk
that unlocks it; if it is ever damaged, the disk is unrecoverable and even the
correct passphrase cannot help. It takes seconds, and it is the one backup that
cannot be made after the fact —
[how to do it](USING.md#disk-encryption-backup-and-recovery).

**How big a drive.** These are guidance and not limits: the installer refuses
only a drive the smallest layout physically will not fit on — a little over
**6 GB** with the current image — and names the exact figure it needed. Under
**8 GB** it warns that updates will not fit and application installs will be
limited; between **8 and 10 GB** that downloads will be limited; above **10 GB**
it says nothing, because there is nothing to say. See
[How big a drive to install onto](USING.md#how-big-a-drive-to-install-onto).

Already running VoidOS and want it on another drive — a USB stick, or a second
internal disk? You can install straight from the system you are using, without a
live USB: see
[Installing VoidOS onto another drive](USING.md#installing-voidos-onto-another-drive).

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

You have three safety nets.

1. **It fixes itself.** If a new version fails to start, your computer notices and
   goes back to the previous one on its own. You do not have to do anything.
2. **You change your mind.** Go to **Settings → Update → Roll back**. The
   previous version is still on the disk, so nothing is downloaded and it only
   takes a restart.
3. **You want to know what is actually wrong.** Go to **Settings → Update →
   Recovery → Check this system**. It looks at the system itself — the copy your
   computer is running, the parts it starts from, the boot menu, and the backup
   of your disk's encryption header — and tells you which of them is not what it
   should be. Checking changes nothing.

   If something is wrong, **Repair** puts back what it can, from the copy of
   VoidOS your computer is already running. Anything it cannot fix from there —
   a damaged copy of the system itself, or the volume your files live on while
   you are using it — it says so and tells you what to do instead, rather than
   pretending. Your files, settings and installed applications are on a separate
   volume and are never touched.

   [What each check means](USING.md#repair).

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

**→ [commands.md](commands.md)** is a plain list of the commands you can type;
**[USING.md](USING.md) is the full guide** — every command with its
subcommands, every keyboard shortcut, and how to install applications.

The system is also its own reference: `voidos help` lists every VoidOS command,
and `voidos help <command>` prints one command's own help. `void` is a shorter
name for the same thing.

| Command | What it does |
|---|---|
| `voidos help` | Every VoidOS command there is. `voidos help <command>` for one |
| `void-settings` | Open Settings |
| `void-disks` | Manage drives and partitions |
| `void-monitor` | See what is running |
| `void-firewall` | Network permissions |
| `void-update` | Check for and install updates |
| `void-get` | Add command-line tools to the machine you are using |
| `void-installer` | Install VoidOS onto another drive — never the one you are running from |
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
install it by name — `flatpak install flathub <id>`, or `--user` for your
account only. Full instructions in [USING.md](USING.md). A proper Apps screen
is planned.

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

## Reinstalling without losing your files

If a machine already has VoidOS on it, the installer offers to **keep your
files and reinstall the system**, and that is the default. Your account,
settings, installed applications and files stay exactly where they are; the
system side is rewritten from the VoidOS you booted, and disk protection stays
as it was. An encrypted disk asks for its passphrase first, or unlocks by its
key if that is how it was set up. **Fresh install** is still there and still
erases everything, but you have to choose it.

Since 0.9.3.10 the installer also keeps a copy of the disk's encryption header
on the boot partition, and every start checks the header before asking for the
passphrase: a damaged one is put back from the copy. It covers a bad sector or
a stray write over the start of the encrypted partition; it does not replace
the off-machine backup, and it cannot recover a lost passphrase.

If a reinstall is not what you need, **Repair** is the smaller answer:
**Settings → Update → Recovery** checks the system, names what is damaged and
puts back what it can from the copy you are running, without touching your
files. See [Repair](USING.md#repair).

---

## Planned releases

Rough order, not fixed dates. Things move when something turns out to matter more.

| Version | What it brings |
|---|---|
| **0.9.3.15** | Tor connection settings — bridges and transports, so Tor works on networks that block it |
| **0.9.4** | Housekeeping and cleanup |
| **0.9.10** | Apps — browse, install and manage applications, in one place |
| **0.9.20** | Firewall — decide what each application is allowed to reach |
| **0.9.30** | Window management — alt-tab, workspaces, and snapping windows to screen edges |
| **0.9.50** | Performance — boot time, memory use and responsiveness, measured before and after |

Also planned, not yet scheduled:

- **Gaming** — getting Steam working; a main use case for the project
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
