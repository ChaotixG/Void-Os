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
- [Getting tools](#getting-tools)
  - [From Settings](#from-settings)
  - [From a terminal](#from-a-terminal)
  - [Where a tool goes, and for how long](#where-a-tool-goes-and-for-how-long)
  - [Tools you kept, seen from Maximum](#tools-you-kept-seen-from-maximum)
  - [Removing a tool](#removing-a-tool)
  - [Two kinds of tool](#two-kinds-of-tool)
  - [What is checked before anything is installed](#what-is-checked-before-anything-is-installed)
  - [Using your own tool channel](#using-your-own-tool-channel)
- [Memory and scratch space](#memory-and-scratch-space)
  - [The scratch tier](#the-scratch-tier)
  - [How much memory a live stick needs](#how-much-memory-a-live-stick-needs)
  - [When memory runs short](#when-memory-runs-short)
  - [What a Maximum session carries](#what-a-maximum-session-carries)
- [Accounts, settings and what a stick keeps](#accounts-settings-and-what-a-stick-keeps)
  - [Settings on a live stick](#settings-on-a-live-stick)
  - [Changing your password](#changing-your-password)
  - [Administrators](#administrators)
- [Installing VoidOS onto another drive](#installing-voidos-onto-another-drive)
- [Reinstalling without losing your files](#reinstalling-without-losing-your-files)
- [Updating from 0.9.3.9 on a machine with 3 GB of RAM](#updating-from-0939-on-a-machine-with-3-gb-of-ram)
- [The first boot after upgrading from 0.9.3.9](#the-first-boot-after-upgrading-from-0939)
- [Updating from a different source](#updating-from-a-different-source)
- [Keyboard shortcuts](#keyboard-shortcuts)
  - [Opening things](#opening-things)
  - [Windows](#windows)
  - [Screenshots](#screenshots)
  - [Hardware keys](#hardware-keys)
  - [Mouse](#mouse)
  - [In the terminal](#in-the-terminal)
- [Commands](#commands)
  - [Getting help on the command line](#getting-help-on-the-command-line)
  - [Applications you can open](#applications-you-can-open)
  - [Screen and session](#screen-and-session)
  - [Sound, screen and input](#sound-screen-and-input)
  - [Appearance](#appearance)
  - [Updates and recovery](#updates-and-recovery)
    - [Repair](#repair)
  - [Disks and network](#disks-and-network)
  - [Security and troubleshooting](#security-and-troubleshooting)
- [Disk encryption: backup and recovery](#disk-encryption-backup-and-recovery)
- [Getting out of trouble](#getting-out-of-trouble)

---

## Installing applications

VoidOS installs applications with **Flatpak**. Flathub — the main Flatpak
catalogue — is already set up, so you do not have to add it. Nothing is
pre-installed; you choose what you want.

Flathub is set up for both system-wide and per-user installs: `flatpak install
flathub <id>` installs for every account and asks for an administrator
password; `flatpak install --user flathub <id>` installs for your account only,
into your own files, with no password.

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

`voidos-build` compiles software from source. It is slower than an app or a
ready-made tool, but reaches things nothing else carries, and it resolves each
build's own dependencies for you.

```sh
voidos-build search nmap         # matches names and descriptions
voidos-build install net/nmap    # the first build sets the system up for you
voidos-build list                # what you have built
```

**No separate setup step.** The build system used to need `voidos-build
setup-user` run once first. Now the first `install` sets it up on its own; the
`setup-user` command is still there if you would rather do it up front, and says
"Already set up" when there is nothing to do.

**It installs into your home folder by default.** To send builds somewhere else
— a bigger volume, say — set it for one command with `VOIDOS_BUILD_PREFIX`, or
keep the choice by putting `prefix=/path` in `~/.config/voidos/build.conf`:

```sh
VOIDOS_BUILD_PREFIX=/mnt/tools voidos-build install security/hydra
```

If you have no persistent storage set up it warns you first, because anything
built would live in memory and be lost at restart — said before the build, not
after.

**Builds use every core**, and your own prefix builds with the same settings as
the system one — one file, `/usr/share/voidos/pkgsrc.mk`, that both read — so a
tool resolves the same dependencies whichever way you build it. A prefix set up
by an earlier VoidOS gets those settings the next time you build or run
`voidos-build setup-user`. A location on a filesystem mounted without execute
permission, such as `/tmp`, is refused up front with the reason, not twenty
minutes in with a compiler's error. Tools installed for your account only are on
your PATH in a console login as well as on the desktop.

**`search` tells you when nothing matches, and where to look instead**, rather
than printing nothing. It searches package names as well as descriptions, so a
tool whose name is not mentioned in any description is still found.

The tree carries the full pkgsrc collection, including its security section —
more than 700 packages there alone: `nmap`, `hydra`, `john`, `sqlmap`, `nikto`,
`hashcat`, `dirb`, `gobuster` and many more are all buildable this way. The
wireless-auditing tools (`iw`, `tcpdump`, the `aircrack-ng` suite including
`airmon-ng`, and `hcxdumptool`) already ship in the image, ready to run.

---

## Getting tools

Applications come from Flathub. **Command-line tools** — a port scanner, a JSON
filter, a file watcher — come from `void-get`, and they install into the machine
you are already using. You do not rebuild anything and you do not reinstall.

### From Settings

**Settings → Apps → Get tools.**

Press **Refresh** once to download the tool list. Nothing is contacted before
you do — VoidOS never fetches a catalogue on its own. Then press **Install** on
what you want. The page tells you what it is doing while it does it.

### From a terminal

```sh
void-get help                   # every command, flag and file, with the rules
void-get help install           # detail on one command
void-get update                 # fetch and check the tool list
void-get search scan            # what is available
void-get info nmap              # everything the list says about one
void-get install jq             # install it
void-get list                   # what you have, and where it lives
void-get remove jq              # take it back off
void-get upgrade                # move every tool you installed to the list's version
```

`void-get help` is the complete reference — every command and flag with a line
each, the files it reads and writes, and what each exit code means.
`void-get help <command>` gives one command in detail, with an example.

### Where a tool goes, and for how long

This is decided by your privacy level, because it is the same question as "what
survives a restart".

| Privacy level | What happens by default | Where a kept tool goes |
|---|---|---|
| **Daily** | The tool is kept. It is still there tomorrow. | Everyone on this machine, if VoidOS is installed. On a live stick with persistence, your home folder — `--device` keeps it for every account on that stick |
| **Secure** | You are asked, every time: keep it, or this session only. | Your home folder, installed or live |
| **Maximum** | This session only. Nothing else is offered. | — |

You can always say which you want:

```sh
void-get install jq --session   # in memory; gone at shutdown, on every level
void-get install jq --user      # your account only; no password needed
void-get install jq --device    # everyone on this machine; asks for the admin password
void-get install jq --keep      # keep it, and let VoidOS pick where
```

`--keep` means "do not lose this", and where it lands depends on the level *and*
on whether VoidOS is installed:

- **Daily, installed** — `/usr/local`, for every account on the machine.
- **Daily, live stick with persistence** — your home folder. Not because a stick
  cannot keep a system-wide tool: your persistence volume carries its own
  `/usr/local` layer, and `--device` writes there and comes back on the next
  boot of that stick. The home folder is simply the default, because it is the
  part that is certain to return, and it needs no administrator password.
- **Secure, either** — always your home folder. Answering "keep" to a yes/no
  question should not quietly turn into a machine-wide install and an
  administrator prompt.

Say `--device` when you mean "everyone on this machine" — on an installed system
or on a stick with persistence, on any level except Maximum.

On **Maximum**, `--user`, `--device` and `--keep` are refused and say so. That is
not a limitation to work around: Maximum's promise is that the machine keeps
nothing, and a tool written to disk would break it.

If you have no persistent storage set up, even a kept tool lives in memory —
`void-get` says so before it starts, not after.

### Tools you kept, seen from Maximum

An installed machine running **Maximum** starts completely fresh every time —
nothing from your last session comes back. Tools you kept earlier on Daily or
Secure are the one exception: `/usr/local` is brought in **read-only**, so your
toolkit is there and runs, while your files, saved networks and passwords are
not. Toolkit present, identity absent.

`void-get list` marks those tools as read-only, and `void-get remove` refuses
them — there is nothing to write to. Switch to Daily or Secure to change them.
Anything you install while on Maximum is a session install and behaves normally.

### Removing a tool

```sh
void-get remove jq
```

It takes back exactly what it put there and says so, line by line: the tool's
own folder, each link it added to your PATH, and its record. A link with the
same name that points at something else is left alone — `/usr/local/bin` belongs
to the machine, not to `void-get`.

For a tool that was **built here**, removing it also removes the packages that
were pulled in only for it, so most of the disk space comes back. Not quite all
of it: that reclaims the libraries the tool needed to *run*, not the tools that
were needed to *build* it — things like `digest`, `libtool-base` and `nbpatch`
stay in the prefix. It is a few megabytes, and the next thing you build reuses
them.

Removing something that is not installed is an error, not a silent success.

The one refusal is on **Maximum**, for a tool kept on another level: there is
nothing to write to. See [above](#tools-you-kept-seen-from-maximum).

### Two kinds of tool

The list says which each one is, and Settings shows it on the row.

- **Ready-made** — a pre-built program, downloaded and put in place. Seconds.
- **Builds here** — compiled on this machine from source, through the same
  pkgsrc system `voidos-build` uses. Minutes to hours, and it resolves its own
  dependencies. Worth it for things no one can ship a portable build of.

When a build finishes, `void-get` tells you the command it installed, because it
is often not the name you asked for — `void-get install netcat` builds a program
you run as `nc`, and it says `type nc, not netcat` rather than leaving you to
guess after an hour's compile. The commands it names are the ones the build
actually installed, read back from it, not the list's guess.

Some tools offer both. `void-get info <tool>` shows the routes it has. By
default the ready-made one is used when it can run on your system; if it needs
a newer glibc than yours, `void-get` says so in one line and builds from source
instead. You can also choose:

```sh
void-get install gobuster --portable   # the ready-made program, or a plain refusal
void-get install gobuster --build      # compile it here instead
```

### Keeping tools up to date

`void-get update` only refreshes the list. To move what you installed forward:

```sh
void-get list                   # a tool that is behind shows  1.8.1 → 1.8.2
void-get upgrade                # every tool that is behind, or name the ones you want
```

An upgrade puts the new version beside the old one and swaps them at the end,
so if anything goes wrong the version you had keeps working. Settings › Get
tools shows an **Update** button on a row that is behind.

### What is checked before anything is installed

The tool list is **signed**, and `void-get` checks that signature against a key
built into VoidOS before it reads a single line. Every download is then checked
against the SHA-256 the signed list names.

If either check fails, nothing is installed and nothing is left behind — you get
an error, not a half-installed tool. A tool built against a newer C library than
your system has is refused up front rather than failing mysteriously later.

On **Secure** and **Maximum** the download goes through Tor, like everything
else. `void-get` waits for Tor to finish starting and shows you its progress,
rather than timing out and blaming the network.

### Using your own tool channel

The tool list comes from one URL, in `/etc/voidos/tools-channel`:

```sh
void-get channel                                   # show it
sudo void-get channel https://example.org/voidos-tools.manifest
```

A mirror is just the files: the manifest, its `.sig`, and the artifacts, all in
one directory. Artifact names in the manifest are resolved next to the manifest
itself, so nothing inside needs editing to move it somewhere else.

If the file is missing, VoidOS uses its built-in default — so an older VoidOS
that has never heard of this file still works.

---

## Memory and scratch space

VoidOS runs almost everything in RAM on purpose, which is what makes a live
session forget itself. The cost used to be that a machine with 4 GB simply ran
out — a big build, a browser with too many tabs, a large capture file, and the
kernel started killing things at random. Two things now sit between you and that.

### The scratch tier

If you have persistent storage — an installed system, or a live USB with a
persistence volume — VoidOS sets up an **encrypted overflow area on disk** and
uses it as swap of last resort. Memory fills first, then the compressed
in-memory swap, and only then this.

What makes it safe to have on a machine built around forgetting things:

- **The key is random and lives only in RAM.** It is read from `/dev/urandom` at
  boot straight into the kernel's encryption table. It is never written to a file
  and never stored on the disk, so there is nothing to find afterwards and nothing
  to seize. Pull the power and last session's contents are unreadable — including
  to VoidOS.
- **It is thrown away and remade at every boot.** The file that backs it is
  deleted and recreated each time, so nothing accumulates across sessions.
- **It costs nothing until it is used.** The file is sparse: it claims space only
  as pages are actually written, and gives that space back as they are freed.
- **It always leaves the volume room to breathe.** The size is whatever is free,
  minus a reserve that scales with the volume: **30% of it, never less than 2 GB
  and never more than 10 GB**. So a 60 GB volume keeps 10 GB back, a 20 GB one
  keeps 6 GB, and an 8 GB one keeps about 2.4 GB — a small disk gets a scratch
  tier instead of being told it is too small for one. If what is left after the
  reserve is under 1 GB there is no scratch tier at all, and VoidOS says so rather
  than filling your disk. `voidos-scratch status` prints the reserve in effect
  alongside the ceiling.

To see what it decided:

```sh
voidos-scratch status     # "active — 12951 MiB ceiling, 5996 MiB kept free, swap …"
                          # or "inactive (why)"
swapon --show             # a partition-type entry at priority 50
```

`voidos-scratch status` is the one to use. `swapon --show` and `/proc/swaps` both
print the device the kernel resolved the tier to — a name like `/dev/dm-2`, not
`/dev/mapper/voidscratch` — so there is nothing recognisable to look for there
beyond the priority: zram sits at 100 and the scratch tier at 50, below it.

`inactive` is a normal answer: a live session without a persistence volume has
nowhere to put it, and a volume with less than a gigabyte left after its reserve
correctly declines. When it declines it says why in brackets.

One thing you may see after an unexpected power loss: if you then boot an older
VoidOS that predates this feature, the tier's backing file stays on the volume,
because only an image that knows about the tier deletes it. It is inert —
ciphertext under a key that died with the power, at most a few hundred megabytes —
and the next boot of a current image removes and recreates it. Nothing needs
doing.

### How much memory a live stick needs

A live session keeps everything it writes in RAM, so on the USB stick — with no
persistence volume — the amount of memory you have *is* your disk space. VoidOS
lets a live session use up to 85% of RAM for this, and those pages are compressed
into RAM-backed swap when they are not in use, so the limit is more generous than
it sounds.

It still has a floor, and it is worth knowing before you plan around it:

| What you want to do | RAM you want |
|---|---|
| Browse, write, use the shipped tools | 2 GB |
| Install a Flatpak application | 3 GB |
| Install a full Flathub **runtime** (a large shared base) | about **4 GB** |

Runtimes are the demanding case because they are downloaded in parts and then
assembled, so the peak is well above the finished size. Measured on a 3 GB
machine, an install failed on the last of six parts reporting it needed 977 MB
free with 556 MB available — a shortfall of about 420 MB, which is why the next
size up is the one to aim for.

If you have less memory than that, the fix is a **persistence volume**: with one,
installed applications go to the volume instead of RAM and this ceiling stops
applying. Settings › Persistence sets one up.

### How big a USB stick

| What you want to do | Stick |
|---|---|
| Run VoidOS from the stick | **4 GB** or more |
| Also keep a persistence volume — settings, files, installed tools | **6 GB** minimum |
| Room for applications and a comfortable working set | **8 GB** or more |

The volume is where everything you keep lives: your files, your settings, and any
tools or applications you install. That is what the extra space buys — a 4 GB
stick runs VoidOS perfectly well, it simply has nowhere to keep anything.

**Updating a stick is not an in-place update.** The A/B update that an installed
system uses needs two system partitions to swap between, and a stick has none —
`void-update` says so rather than pretending. You update a stick by writing the
new image to it, exactly as you wrote the first one. **Your persistence volume is
kept**: it is a separate partition, the writing tools replace only the image, and
the new system finds the volume by its label as before. Nothing you have saved is
lost in an update, and you do not need to back the volume up first.

**Your account is remembered.** The first time you boot a stick that has a
persistence volume, setup runs as usual and asks you to create an account. That
account is then saved onto the volume — the name, the password and what it is
allowed to do — and every later boot goes straight to the login screen. Setup
runs once, not every time. This is needed because a live stick carries your home
folder on the volume but not `/etc`, which is new at every boot; without the
saved record, setup asked you to recreate an account whose files were already
sitting there.

On *Maximum* nothing is saved and nothing is restored: that level starts from
nothing by design, so VoidOS refuses to write the record at all, setup runs each
boot, and a record saved on another level is left untouched on the volume for
when you switch back.

**One stick is enough.** The image uses about 1.6 GB, and the rest of the stick
is free space that can hold the persistence volume — Settings offers it as
*"This stick's free space"*, at the top of the device list, whenever there is at
least 1 GB spare. You do not need a second stick. The volume is added after the
image without touching it: the ISO stays byte-for-byte as it was written, and the
stick still boots on both BIOS and UEFI machines.

The same rule applies to VoidOS **installed** on a disk: the installer keeps
3 GB free on the persistence volume for exactly this reason, whenever the drive
is big enough to spare it.

### How big a drive to install onto

These are guidance, not limits. The installer will use any drive the system
physically fits on — a little over **6 GB** with the current image — and tells
you what a smaller one will cost before it does anything.

| Drive | What to expect |
|---|---|
| under **8 GB** | Installs and runs, but there is no room to stage a system update, and installing applications is limited |
| **8–10 GB** | Installs and runs normally; downloads and applications have less room than usual |
| **10 GB** and up | No compromises |
| about **15 GB** and up | Full-size 4 GB system slots, so the image can grow across releases without the layout changing |

The installer adapts the layout to the drive rather than refusing it: on a small
drive the system slots are sized to the image instead of to a fixed 4 GB, the
sessions volume shrinks, and the update staging room is dropped — in that order,
because every megabyte not spent on those goes to your own files instead. It
refuses only when even the smallest layout will not fit, and then it says exactly
how much space it needed and where it went:

```
/dev/sdb is 6144MB and the smallest layout VoidOS can write needs 6164MB:
an EFI and a boot partition (1556MB), two 1920MB system slots for a 1530MB
image, 256MB of sessions and 512MB for your files. Nothing has been changed.
```

The figure moves with the image, because it is derived from it: each system slot
is the image rounded up by a quarter, and there are two of them. The disk picker
in the installer works it out the same way and greys out a drive below it, so
you are told before you choose rather than after.

### When memory runs short

`void-psi` watches how much time the machine is spending stalled on memory and
steps in before the kernel's out-of-memory killer would — which is worth doing,
because that killer chooses by a heuristic and has been observed taking out the
desktop, the Tor daemon and the login manager while leaving the program actually
responsible running.

Applications you launch from the desktop each get their own accounting group, so
"which program is eating the machine" has an answer. The response escalates only
as far as it has to:

1. **Slow it down.** The largest application is throttled: the kernel starts
   reclaiming memory from it and refuses to let it grow. Nothing is closed and
   nothing is lost; the machine stays responsive. This is reversed automatically
   once pressure has been low for half a minute.
2. **Ask it to close.** If the machine is still stalled, its largest process is
   asked to quit, and you get a notification naming it.
3. **Force it.** If it does not go, it is force-quit. This is the last step, and
   it exists so the kernel's killer never runs.

The compositor, the panel, the system services and Tor are never candidates at
any step. Everything is logged with the application's name and the numbers behind
the decision, in `/var/log/void-psi`.

### What a Maximum session carries

On Maximum, VoidOS forgets — and it now forgets on an **installed disk** just as
thoroughly as on the live USB.

**On the live USB.** The boot menu offers five profile entries — *Daily
(Recommended)*, *Secure*, *Maximum*, *Secure with Persistence* and *Maximum with
Persistence (tools only)*. *Daily* is the one the menu starts on, so pressing
Enter gives you Daily. The last two are the entries that open a persistence
volume. Below them the menu also carries *DEBUG*, *DEBUG safe-graphics*,
*Recovery (AppArmor off)* and *Memory Test*, which are for diagnosing a machine
rather than using one.

Choosing plain **Maximum** gives you a session that keeps nothing at all: no home
folder, no saved networks, no Tor state, and no volume opened. That is the
strictest thing the stick can do, and it is what the entry means.

**Maximum with Persistence (tools only)** is the middle position, and it is worth
understanding precisely. The volume is opened, but only your **toolkit** is taken
from it: `/usr/local` is layered in read-only, so the tools you installed with
`void-get` on another level are there to run. Nothing that carries identity is
touched — no home directory, no Tor state, no saved Wi-Fi passphrases, no flatpak
store — and the saved account is not restored either, so you get the first-run
setup and a fresh throwaway user exactly as on plain Maximum. The volume itself is
mounted `0700 root` and `nosuid,nodev,noexec`, so the session user cannot walk
into the data stored on it — and Settings reports the session as *"Tools only —
Maximum: nothing else is kept"* rather than claiming persistence is on. Choose it
when you want your tools without your identity; choose plain *Maximum* when you
want the stick to open nothing at all.

One side effect worth knowing: a stick booted this way carries a changed
permission on the volume's top directory. The next *Daily* or *Secure with
Persistence* boot puts it back automatically, so nothing is lost either way.

If you want your files on the stick, boot *Secure with Persistence*; if you want
Maximum's rules applied to a machine that also has your data, that is what an
installed system gives you, below.

**An installed system set to Maximum.** The disk is still there and still
encrypted, but the machine boots amnesic: the whole root filesystem runs in
memory, so your accounts, files, settings, keyring and saved networks are not
mounted anywhere in the session — not even read-only. You get the same first-run
setup you would on a fresh stick, and a fresh user each boot. The device-scope
toolkit is presented read-only — tools you installed on Daily or Secure are
still there to use — and the encrypted scratch tier still works. Nothing else on
the disk is reachable from the session.

Two consequences worth knowing before you choose it:

- **Anything you make during a Maximum session is gone at shutdown.** Save it to
  external storage before you power off.
- **You can still leave.** Change the level in Settings and it is written to the
  disk immediately, so the next boot comes back with all your files. Switching to
  Maximum and switching away are both one reboot.

Daily and Secure are unchanged on both a stick and an install: everything
persists as it always has.

---

## Accounts, settings and what a stick keeps

### Settings on a live stick

On a live stick with a persistence volume, the settings the system keeps for
itself are now written to the volume and are still set the next time you boot
that stick. Before, they were kept in memory: the page showed the new value, and
the next boot answered with the shipped default without saying so.

What this covers:

| Setting | Where you change it |
|---|---|
| The tool channel | `void-get channel <url>` |
| Whether updates are checked automatically | Settings → Update |
| The firewall's network mode | Settings → Firewall |
| The confinement mode | Settings → Confinement |
| Hardware-address randomisation | The **Network identity** section in Settings |
| Power mode, charge limit, lid action | Settings → Power & battery |
| Screen brightness | The brightness keys; put back at the next boot |
| Keyboard layout | Setup, or `voidos-keyboard` |
| Per-application device permissions | Settings → Apps → App permissions |
| Your keyring | Set up at your first sign-in |

**Your privacy level is deliberately not one of them.** On a stick, the boot
menu entry you choose decides it, every time — that is what the entry is for. A
level chosen during a live session applies to that session and is not written
anywhere. On an installed machine the level is a recorded choice as before.

**Maximum keeps none of this**, including on the *Maximum with Persistence
(tools only)* entry. The volume is open there and this is still not read, so a
setting stored on the volume can never loosen a Maximum session. See
[What a Maximum session carries](#what-a-maximum-session-carries).

**Settings → Storage & persistence tells you which it is.** With the volume open
it says
either that settings are written to the volume too, or that they are kept in
memory only and will return to their defaults at the next boot.

None of this applies to an installed machine, where `/etc` has always been on
the disk.

### Changing your password

The **Change password** button in **Settings → Accounts** opens a terminal
running `passwd`. On a
stick with a persistence volume the new password is written to the volume a few
seconds later, on its own.

This used to happen only when the machine shut down cleanly, so a stick that
lost power came back wanting the old password — the one thing the saved account
exists to prevent. It now happens whatever changed the password, because what is
watched is the password file itself rather than any one command.

The change itself is untouched: the saving is a separate step that reacts
afterwards and cannot delay or fail a password change. On *Maximum* nothing is
written, as everywhere else.

### Administrators

**Settings → Accounts** can grant and revoke administrator. Doing so now also
updates which account VoidOS asks for when you change the privacy level — before,
that could go on naming someone who was no longer an administrator, whose
password would still authorise a change and whose account could not be deleted.

**Revoking the only administrator is refused.** Nothing would be left that could
authorise a change afterwards, including changing it back. Make someone else an
administrator first.

**Every account is remembered, not just the one you set up first.** On a stick
with a persistence volume the saved record holds all of your accounts, so adding
a second account, promoting it and demoting the first all come back correctly at
the next boot. If a machine ever did reach a boot with no administrator at all,
VoidOS gives administrator back to the account you created first and says so,
rather than leaving a stick that can authorise nothing — so you cannot lock
yourself out this way.

---

## Installing VoidOS onto another drive

You do not need the live USB to install VoidOS again. From the system you are
already running, you can install it onto a **different** drive — a USB stick you
want to carry with you, or a second internal disk.

Tap **Super**, type **Install VoidOS**, and open it.

**It installs the version you are running.** Nothing is downloaded, and you
cannot end up with a version older or newer than the one you are sitting in
front of.

**The drive you are running from is never offered.** It does not appear in the
list at all, so there is no way to pick it by mistake and no way to wipe the
system you are using.

**Everything on the drive you choose is erased.** Before anything is written,
the installer asks you to type the name of the drive you picked, exactly as it
is shown on screen. Nothing starts until you do, so a mis-click or a stray Enter
cannot begin an install.

**A computer can hold more than one VoidOS.** Each install is complete and
independent — its own account, its own files, its own privacy level, and its own
updates. Which one you get depends on which drive you start the computer from.

**The new install starts clean.** The first time you boot it you land on the
normal login screen and sign in with the account you created in the installer.
It is a fresh system, not a copy of the one you installed from.

Installing onto a second internal disk is a perfectly reasonable thing to do.
Just remember it is erased like any other drive — look at what is on it before
you choose it.

---

## Reinstalling without losing your files

Boot the live USB, open **Install VoidOS**, and pick the disk that already has
VoidOS on it. The installer notices and offers **Keep my files and reinstall the
system**, which is selected by default:

- **Kept:** your account and password, your files, settings, installed
  applications, saved networks, and the security profile.
- **Rewritten:** both system slots, the kernel and bootloader, and the session
  volume (which never holds anything of yours).
- **Disk protection:** stays as it was. A passphrase-protected disk asks for its
  passphrase before anything happens; a disk that unlocks automatically does so
  by its key; an unencrypted disk needs nothing. A wrong passphrase stops the
  reinstall with nothing changed.

Choose **Fresh install** instead if you want everything erased. It says so
before it does anything, and asks you to type the disk's name to confirm.

Reinstalling cannot change how the disk is protected; that needs a fresh
install.

## Updating from 0.9.3.9 on a machine with 3 GB of RAM

If you are on **0.9.3.9** and the machine has **3 GB of RAM or less**, the
update to 0.9.3.10 will not install. It stops before it starts, with:

```
void-update: not enough space to stage the update. need about 1530 MiB (plus
margin), 1451 MiB free in /run/voidos
```

**Nothing is written and nothing is damaged.** The update is refused before the
download begins, so the machine you are sitting at is exactly as it was.

**Why it happens.** The 0.9.3.9 updater staged the downloaded system image in
`/run/voidos`, which is memory, not disk: `/run` is a tmpfs capped at half of
the machine's RAM. On a 3 GB machine that is about 1451 MiB, and the 0.9.3.10
image is 1530 MiB — so it does not fit, whatever is free on the disk. A machine
with 4 GB or more normally has the room, though at exactly 4 GB it was close
enough to go either way.

**The way through: reinstall from the 0.9.3.10 USB stick, keeping your files.**
Write the 0.9.3.10 image to a stick, boot it, open **Install VoidOS**, and pick
the disk that already has VoidOS on it. The installer offers **Keep my files and
reinstall the system**, which is selected by default, and says so before it
starts:

```
Reinstalling VoidOS on /dev/sda — your account, files, settings and installed
applications are kept
```

and when it is done:

```
Reinstall complete — VoidOS 0.9.3.10 is on /dev/sda. Your account, home
directory, settings, installed applications and saved networks were kept; the
system, the bootloader and the session store were replaced.
```

An encrypted disk asks for its passphrase first, or unlocks by its key if that
is how it was set up. See
[Reinstalling without losing your files](#reinstalling-without-losing-your-files)
for exactly what is kept and what is rewritten.

**This cannot happen to you again.** From 0.9.3.10 onward an installed system
stages the update on the persistent volume, which is on the disk and has room
measured in gigabytes, and falls back to memory only on a live stick. The size
of your RAM stops being part of whether an update can install.

## The first boot after upgrading from 0.9.3.9

If your machine did take the update from **0.9.3.9**, its first 0.9.3.10 boot
runs on the **previous startup image**. The old updater copies a kernel and
startup image onto `/boot` only when the name is new, and the kernel's release
name did not change between the two versions, so it copied nothing and said it
had.

**There is nothing to do about it.** The step that confirms a good boot runs at
every start, and it now brings the running copy's kernel and startup image up to
date from the system image you are running. What it replaces is kept alongside
as `.prev`, and a missing disk-header backup is rebuilt at the same time. So your
**second** boot already runs this release's startup image, and
`void-update verify` reads `ok` without a command being typed.

On an ordinary boot it compares the two and writes nothing, because they already
match, and it says nothing. It never delays or undoes the boot being confirmed:
that has already happened by the time it runs. If `/boot` is short of space it
warns, leaves everything exactly as it was, and the boot is still confirmed.

**One thing to know about rolling back.** Until your next update both system
copies name the same kernel pair — that is what the update into this release
leaves behind — and `void-update verify` reports it as *shared* rather than
damaged. The next update gives each copy a pair of its own. From then on the
copy you can roll back to is the other 0.9.3.10, not 0.9.3.9: the older release
is no longer on the disk.

## Updating from a different source

Updates are fetched from the address in `/etc/voidos/update-channel`, one line
holding the URL of a release manifest (`voidos-update.json`). To use a mirror
or your own server, put its manifest URL there as root. A mirror is a copy of
the release files plus a manifest that names the image and its signature by
filename; they are then fetched from beside the manifest. Every image is still
checked against the signing key built into the system before it is written,
whatever the source, so a mirror can only serve genuine releases.

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

### Getting help on the command line

The tables below are a short list of the useful ones. The complete list lives in
the system itself, and every command in it can explain itself:

| Command | Does |
|---|---|
| `voidos help` | The catalogue: every VoidOS command there is, grouped by what it is for, one line each |
| `voidos help <command>` | The help for one command, without having to remember where it lives |
| `<command> help` | The same thing, asked directly. `--help` and `-h` work too |
| `<command> help <subcommand>` | The detail for one subcommand, where a command has them |
| `voidos version` | The version you are running |

`void` is a shorter name for the same thing, so `void help` and
`voidos help` are identical. There is also a plain reference of the commands you
are likely to type in [commands.md](commands.md).

Each command's help says what it does, every option it accepts, every file it
reads or writes, and what each exit code means. Commands that are not meant to
be typed — the ones VoidOS starts for itself — say so, and say what starts them.

Asking for help never does anything: it prints and exits, so it is safe on any
command whatever, including the ones that erase disks.

**A command that fails tells you why.** Give one an argument it cannot use, or
leave out one it needs, and it says exactly what was wrong — not a wall of help
text. The full help appears only when the command name itself is not one VoidOS
knows.

### Applications you can open

| Command | Opens |
|---|---|
| `void-settings` | Settings |
| `void-monitor` | System Monitor |
| `void-disks` | Disks |
| `void-firewall` | Firewall |
| `void-installer` | Install VoidOS onto another drive — never the one you are running from |

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

Updating from 0.9.3.10 to 0.9.3.11 is an ordinary update: it is staged on the
disk rather than in memory, and it carries its own kernel and startup image, so
there is nothing to do beyond **Settings → Update → Check now → Install** and a
restart.

Two things the updater says differently since 0.9.3.11. It now names the
temporary `.new` and `.prev` copies it clears out of `/boot` after a good boot,
in `/boot/voidos-update.log`, instead of removing them silently — so being told
"the previous kernel is kept as `.prev`" and later finding it gone is no longer
a mystery. And `void-update status`, after you roll back an update that was
staged but never started, says the boot menu points at the slot already running
and the next start changes nothing, rather than reporting a pending update that
would fall back to itself.

| Command | Does |
|---|---|
| `void-update help` | Every command and option, the files it touches, and what each exit code means. Run with no command at all it prints the same text and stops |
| `void-update status` | Which version you are on and what is on the spare slot |
| `void-update rollback` | Go back to the previous version at the next restart |
| `void-update verify` | Check this installation for damage. Reads only; changes nothing |
| `sudo void-update repair` | Put back what is damaged, from the system image you are running |
| `sudo void-update fsck-persist <disk>` | Repair the volume your files live on. Run it **from the VoidOS USB stick**, naming the machine's disk — it refuses a disk the session is running from |

Use **Settings → Update** for normal updating — it is the same thing with a
progress bar.

#### Repair

Rolling back and reinstalling used to be the only two answers to "this machine
has gone wrong". Both are large responses to one file having gone bad.
**Settings → Update → Recovery → Check this system** says what is actually
wrong, one line per thing checked, and changes nothing while it looks.

`void-update verify` checks:

| It checks | And says |
|---|---|
| The system image in the slot you booted | Whether it is byte-for-byte the image that was installed |
| The kernel and the startup image on `/boot` | Whether they are the ones inside that system image. From the first update made **by** 0.9.3.10 onward each slot keeps its own pair, named for the version it holds, so an update never overwrites the other slot's and going back always finds the kernel it was built with. The update **into** 0.9.3.10 is the exception — it was performed by the older updater, which names one pair for both slots; VoidOS reports that as *shared*, not damaged, and the next update ends it |
| The bootloader on the EFI partition | Whether it is the one this version ships |
| The boot menu | Whether both entries are complete and name the right disk |
| The disk-encryption header backup | Whether it exists and belongs to *this* disk |
| The volume holding your files | Whether its filesystem is clean, when it can be checked safely |
| The update signing key and the update channel | Whether this machine can still be updated at all |

Each line says `ok`, `damaged` or `unknown`. **`unknown` is not a failure.** An
installation made before 0.9.3.10 never recorded what it installed, so its
system image cannot be compared. Neither can one whose record was left behind by
something that rewrote a slot without updating it — an older version of the
updater, or a copy made by hand; VoidOS says so and compares nothing rather than
calling bytes damaged on the strength of a record that does not describe them.
And several checks need administrator rights, which the check deliberately does
not ask for. All of these say so, and the page offers a **Check as
administrator** button so the choice is yours.

**Repair** becomes available when something is damaged. It puts back, in this
order and only what differs:

1. the kernel and the startup image on `/boot`, from the system image you are running;
2. the bootloader on the EFI partition;
3. the boot menu, rebuilt from that image's template;
4. the disk-encryption header backup, checked against your disk before it replaces anything.

Your files, settings and installed applications are on a different volume and
are never touched by any of this. Running repair twice changes nothing the
second time.

**What repair cannot do from here, and says so instead of pretending:**

- **A damaged system image.** The slot you are running is mounted as the system
  itself and cannot be rewritten underneath you — and nothing may be copied out
  of it either, or the damage would be written onto `/boot` rather than off it.
  VoidOS writes a fresh image into the *spare* slot instead and boots it next
  time, which is exactly what an update does. It uses the update channel when
  that channel offers the version you are already on, or files you supply:
  `sudo void-update repair --from /path/to/voidOS-…squashfs /path/to/…squashfs.sig`.
  A local file is checked against the same signature as a download; an unsigned
  or wrongly signed image is refused either way. If the channel offers a *newer*
  version, VoidOS tells you to install that update instead rather than quietly
  changing which version you run.
- **A missing boot menu.** `/boot/grub/grub.cfg` carries the GRUB password, and
  that password exists in no other copy, so it cannot be regenerated. VoidOS
  points you at `/boot/grub/grub.cfg.prev`, kept automatically, or at
  reinstalling from the USB stick — which keeps your files.
- **The volume holding your files, while you are using it.** A filesystem repair
  needs the volume unmounted, and the running desktop is writing to it — a repair
  pass underneath a live filesystem is how a recoverable volume becomes an
  unrecoverable one. VoidOS checks it automatically before mounting it at every
  boot. To repair it by hand, start the **VoidOS USB stick** and run

  ```sh
  sudo void-update fsck-persist /dev/sda     # the machine's own disk
  ```

  It finds the VoidOS layout the way the installer does, unlocks an encrypted
  disk with the key on its boot partition or by asking for the passphrase,
  repairs the volume, and locks everything again on the way out. It refuses any
  disk this session is running from, and any disk with something mounted on it,
  so it cannot be pointed at the stick by mistake. `e2fsck` sometimes finds
  damage it will not guess at unattended; when that happens VoidOS says so and
  gives you the exact command to run.

### Disks and network

| Command | Does |
|---|---|
| `void-disk` | Disk helper used by the Disks application |
| `void-network` | Network helper used by Settings |
| `voidos-luks-backup save <device> <folder>` | Back up your disk-encryption header. See [Disk encryption](#disk-encryption-backup-and-recovery) — this is the one backup that cannot be replaced later |

### Security and troubleshooting

| Command | Does |
|---|---|
| `voidos-confine status` | How many applications are confined, and how |
| `voidos-confine check firefox` | Is this program confined, and by what |
| `voidos-confine denials` | What was recently blocked — the first thing to look at when an app misbehaves on Maximum |
| `sudo cat /run/voidos/install.log` | Every step and warning from the last install, including which layout it chose and why. Written fresh each run and kept only until you power off; the passphrase and the account passwords are read separately and never reach it |
| `sudo cat /run/voidos/persist-create.log` | What the last persistence-volume creation did, including which device it wrote to. Written fresh each attempt and kept only until you power off; it never contains your passphrase |
| `voidos-build` | Install software from source (see above) |

### Tools

| Command | Does |
|---|---|
| `void-get help` | Every command, flag and file, with what each exit code means. `void-get help <command>` for one in detail |
| `void-get update` | Fetch the signed tool list. Nothing is contacted until you ask |
| `void-get search <text>` | What is available. No text lists everything |
| `void-get info <tool>` | Version, kind, checksum and what it puts on your PATH |
| `void-get install <tool>` | Install it. Add `--session`, `--user`, `--device` or `--keep` to say where |
| `void-get remove <tool>` | Remove exactly what was installed — files, PATH links, and for a source build its leftover dependencies. Refused on Maximum for a tool kept on another level |
| `void-get list` | What void-get installed, with the scope each one is in, and which are read-only |
| `void-get channel [url]` | Show, or (with `sudo`) set, where the tool list comes from |

See [Getting tools](#getting-tools) for what the scopes mean on each privacy
level.

Most `voidos-confine` subcommands need `sudo`.

---

## Disk encryption: backup and recovery

If you chose an encrypted disk, read this once now — not when you need it.

### Why this matters more than a normal backup

Your disk is unlocked by a **header** stored at the start of it. That header
holds your key, wrapped by your passphrase. If those few megabytes are damaged —
a bad sector, an interrupted write, a mistyped command aimed at the wrong device
— **the disk is gone.** Not difficult to recover: gone. The correct passphrase
cannot help, because the thing it unlocks no longer exists.

A header backup is the only defence. It takes seconds.

### Making the backup

Plug in a USB stick, and:

```sh
sudo voidos-luks-backup save /dev/sda2 /run/media/you/STICK
```

Use the partition that holds the encryption, not the whole disk. `lsblk -f` shows
which one says `crypto_LUKS`.

It prints where it saved the file and its checksum. Keep both.

Three things it will tell you, and all three matter:

- **It refuses to write the backup onto the disk it protects.** A header backup
  stored on the encrypted disk protects nothing.
- **If your USB stick is FAT or exFAT it warns you and asks for confirmation**,
  because those filesystems have no permissions — the file would be readable by
  anyone who picks the stick up.
- **The backup contains your key slots.** Anyone holding this file *and* a
  passphrase that was valid when it was made can decrypt the disk — including a
  passphrase you revoke later. Store it offline, treat it like the disk itself.

To see what is currently on a device: `sudo voidos-luks-backup show /dev/sda2`.

### Restoring after a damaged header

**You cannot do this from the broken machine** — it will not boot. You need the
VoidOS USB stick you installed from, or any live Linux.

1. Boot the **live USB**, not the installed system.
2. Plug in the medium holding your header backup.
3. Make sure the encrypted volume is **not unlocked**. If you were prompted for
   your passphrase and entered it, restart and skip the prompt. Restoring a
   header underneath an unlocked volume leaves it permanently unopenable — the
   tool refuses to do it, and that refusal is protecting you.
4. Restore:

```sh
sudo voidos-luks-backup restore /dev/sda2 /path/to/voidos-luks-header-sda2-TIMESTAMP.img
```

It will state what it is about to overwrite and ask you to type `RESTORE`. Then
restart normally and unlock with your passphrase.

### The two ways this goes wrong

**Restoring a header from a different disk destroys everything on the target,
permanently.** The header carries the key for *its* disk; put it on another one
and every byte becomes unreadable, with no recovery. The tool records which
volume each backup belongs to and refuses a mismatch — which is why you should
keep the small `.uuid` file saved alongside the backup. If it cannot check, it
says so, and you should stop and be certain.

**Your passphrase goes back in time.** The restored header is the one from the
day you made it. Any passphrase you added *since* will not work, and any
passphrase you removed since **will work again**. If you changed your passphrase,
make a fresh backup and destroy the old one.

### Automatic header backup and recovery

Since 0.9.3.10 the installer keeps a copy of the disk's encryption header on
the boot partition, for both passphrase and automatic-unlock installs, and a
reinstall refreshes it. At every start, before the passphrase prompt, VoidOS
checks the header on the disk; if it is damaged beyond what the encryption
itself can repair, the copy is put back and the normal unlock runs. If no
backup is found or the copy cannot be restored, nothing is touched and the
unlock fails visibly, as it would have anyway.

What this does and does not protect against:

- It recovers from a **damaged header** (a bad sector, a write that hit the
  wrong place, a tool that wrote over the start of the partition).
- It does **not** protect against a lost passphrase or key, and it does not
  weaken the encryption: the copy on the boot partition is the same header that
  already sits on the encrypted partition, and it is useless without your
  passphrase or key.
- It is **not** a substitute for the off-machine backup described above. A
  machine whose whole drive fails, or whose boot partition is damaged too, still
  needs the copy you saved elsewhere.
- If you ever change the disk passphrase by hand with `cryptsetup`, the copy on
  the boot partition still holds the **old** key slots. Refresh it afterwards:

```
sudo rm -f /boot/voidos-luks-header.img
sudo cryptsetup luksHeaderBackup /dev/sda3 --header-backup-file /boot/voidos-luks-header.img
sudo chmod 0400 /boot/voidos-luks-header.img
```

  (use your own encrypted partition in place of `/dev/sda3`). A reinstall
  refreshes it as well.
- With two VoidOS drives in one machine, the copy used is always the one on
  the same drive as the boot partition being started; a copy from another drive
  is never applied.

---

## Getting out of trouble

**An application will not start on Maximum.** Run `sudo voidos-confine denials`
to see what was blocked. Switching to Secure in Settings is the quick answer.

**The screen looks wrong after changing settings.** `voidos-theme apply`.

**An update went badly.** **Settings → Update → Roll back**, then restart. The
previous version is still on the disk, so nothing is downloaded. If the machine
will not start at all, it goes back on its own.

**Something about the system itself seems wrong.** **Settings → Update →
Recovery → Check this system**, or `void-update verify` in a terminal. It names
what is damaged rather than leaving you to guess, and **Repair** puts back what
can be put back without touching your files. See
[Repair](#repair).

**Something is frozen.** Hold the power button for about five seconds.

**You want to check your disk-encryption backup.** `sudo voidos-luks-backup show
/dev/sda2` prints what is on the device now. Making a backup, and restoring one
after damage, are covered in
[Disk encryption](#disk-encryption-backup-and-recovery).
