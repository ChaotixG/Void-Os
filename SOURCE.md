# Obtaining source code

A VoidOS image contains software licensed under the GNU General Public License,
the GNU Lesser General Public License, and other licenses that entitle you to
the corresponding source code. This document explains how to get it.

This obligation is real and the VoidOS Project honours it. If anything here does
not work for you, that is a bug — please report it.

## What is covered

Every third-party component in a VoidOS image whose license requires source
availability. That includes, among many others:

| Component | License |
|---|---|
| Linux kernel | GPL-2.0 |
| GNU Bash, coreutils, grep, sed, tar | GPL-3.0 |
| GNU C Library (glibc) | LGPL-2.1 |
| GTK, GLib, Pango, Cairo | LGPL-2.1 |
| labwc (the compositor) | GPL-2.0-only |
| wlroots, swaylock, swayidle | MIT |
| Tor | BSD-3-Clause |

It does **not** cover the VoidOS Original Works, which are proprietary — see
LICENSE, Section 1. Those are not GPL-licensed and no source obligation attaches
to them.

## Exactly which versions

Every third-party component is pinned by version, upstream URL and SHA-256 in
`sources.lock`, published with each release. That file is the authoritative
record of what went into a given image: it names the precise upstream release
each binary was built from, so you can fetch it from its original author and
verify it matches what VoidOS used.

## Written offer

For any VoidOS release, and for three years from the date that release was
published, the VoidOS Project will provide, to any third party, the complete
corresponding machine-readable source code for the copyleft-licensed components
in that release — including any VoidOS-applied patches and the build
configuration used to compile them — for no more than the cost of physically
performing the distribution.

To request it, open an issue at
<https://github.com/ChaotixG/Void-Os/issues> stating the release version, or
contact the project directly. Requests are answered.

## Modifications

Where VoidOS patches a copyleft component, the patch is part of the
corresponding source and is supplied with it. VoidOS aims to ship upstream
releases unmodified wherever possible; build configuration (compile flags,
feature toggles) is recorded in the build definitions supplied under the offer
above.
