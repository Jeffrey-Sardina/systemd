# Liberated `systemd`

Mass surveillance is bad, actually. So here's a fork of `systemd` with surveillance enablement removed, which will be kept up-to-date with other changes in `systemd/main`. However you use this, or do not, is your choice and yours alone.

## Purpose
The purpose of Liberated `systemd` is to do exactly one thing, and do it well: removing surveillance enablement from base `systemd`. Specifically, here is what I mean by surveillance: **surveillance is the tooling that enables or facilitates collection of any personal information that does not arise from technical needs for `systemd`**. The primary offender of this is, of course, age verification. If `systemd` later adds in support for other surveillance mechanisms, those will also be removed.

What this also means is that Liberated `systemd` is not a divergent development project. It will not introduce new features, correct bugs or security issues, or implement optimizations. If you want to contribute to any of those things, the correct way to do so is to raise a PR against the base `systemd/systemd` repo. This repo exists only to remove surveillance enablement.

## Installation
Detailed blog post: https://medium.com/@jeffrey.sardina/installing-liberating-systemd-a-guide-by-the-author-2a9823192dd3

### Risks (read this!)
In short, first,you must understand the risks. Liberated systemd currently is, essentially, a nightly build. This means:
- it might not be as stable as a named release of base systemd
- it might have new, changed, or deprecated features relative to the most recent named releases of base systemd
- therefore, there is potential for version mismatches with what your OS expects, as the nightly version is likely newer than that named release most distros install

Further, Systemd is the first process to run on your computer -- if you corrupt it, your computer will not boot. So whenever you go about changing the installed version -- be very careful. This means:
- record everything you do
- have backups of all your data
- have a live-bootable USB / CD / etc just in case
- go slowly, and understand the effects of all commands before you run them
- Don't install anything else at the same time as Liberated systemd, and reboot after you install. This way, if you run into issues, you can track down what caused it more easily. Only install other programs once you've verified Liberated systemd is working correctly on a fresh boot.
- make sure your distro is using systemd! *Almost* all Linux distros do (Debian, Ubuntu, Arch, Fedora, Red Hat, etc). But some distros, such as Artix and Void, use other init systems (and are anti-surveillance already). Liberated systemd only matters if your distro runs systemd.

Everyone's local setup is different, and what works for one personmight not work for you. I highly recommend test-installing in a VM before running on your actual computer.

### Install from source
Here is the set of commands you can use to install Liberated systemd from source. This is how I run Liberated systemd on my Arch and Ubuntu (26.04 / "resolute") systems, although the commands should work on any Linux distro. First, we check the current version of systemd:

```bash
systemctl --version # output will contain something like "systemd 259 (259.5-0ubuntu2)"
```

You'll also need to know your OS release:

```bash
cat /etc/*-release # see the line DISTRIB_RELEASE=<release>
# in some cases (such as Ubuntu) you instead must use DISTRIB_CODENAME=<release>
```

Then, we pull and build Liberated systemd from source:

```bash
# pull Liberated systemd source code
git pull https://github.com/Jeffrey-Sardina/systemd.git

# pull mkosi -- latest version
git clone https://github.com/systemd/mkosi.git

# build the newimage with MKOSI
# your distribtuion must beone of: fedora,debian,kali,ubuntu,postmarketos,arch,opensuse,mageia,centos,rhel,rhel-ubi,openmandriva,rocky,alma,azure,custom
# replace <distribution> and <release> with their correct values
# this may take some time
cd systemd/
../mkosi/bin/mkosi -d <distribution> -r <release> -t none -f # see: https://systemd.io/HACKING/
```

Finally, we install the built packages (again, see https://systemd.io/HACKING/). The command differs based on your OS:

```bash
# Fedora/CentOS
run0 dnf upgrade build/mkosi.builddir/<distribution>~<release>~<architecture>/*.rpm

#Debian/Ubuntu
run0 apt-get install build/mkosi.builddir/<distribution>~<release>~<architecture>/*.deb
# the ./ is needed to tell apt to look for local files, rather than online ones

# Arch Linux
run0 pacman --upgrade --needed --noconfirm build/mkosi.builddir/<distribution>~<release>~<architecture>/*.pkg.tar

# OpenSUSE
run0 zypper --non-interactive install --allow-unsigned-rpm build/mkosi.builddir/<distribution>~<release>~<architecture>/*.rpm
```

Finally, check the version again to verify you have upgraded:

```bash
systemctl --version # output should be different than the previous version check
```

Reboot your computer, and congratulations! Your computer is now liberated from age verification enablement!

**Note that installing this way won't have automatic upodate. You'll need to update manually to keep your system secure with the latest security patches.

### List of community repos
There are community-made packages you can install (aside - I'm stunned that others have done this, and absolutely love it!). That said, I did not create these, have not tested them, and cannot take credit (or responsibility) for them - so make sure you understand what they are doing before you install them. But if you'd like, here is a list of the ones I am aware of:
- For Arch (via AUR): https://aur.archlinux.org/packages/systemd-liberated-git (Credit: [zeakz](https://aur.archlinux.org/account/zeakz))

## How often is this updated (or, "why is Liberated `systemd` behind by X commits?")
Liberated `systemd` will be updated at least weekly. Note that the base `systemd` repo is updated very frequently -- typically 20-30 commits in a day. Many of these come from merging PRs with multiple commits in their history. As a result, it's quite common to see Liberated `systemd` behind by 50 or more commits -- even when its code is only a few days behind. So check on the commit dates as well if you want to know how up-to-date Liberated `systemd` is.

I do currently have a setup that would allow automating these updates. I have so far held back from full automation, however, since I prefer to scan new commits manually to make sure there are no more surveillance-enabling changes. So far this approach has worked well. If the manner in which I maintain this changes, I'll update here.

## How is Liberated `systemd` implemented?
It's quite simple: `systemd`, very nicely, has (mostly) atomic commits. There is exactly one commit (https://github.com/systemd/systemd/commit/acb6624fa19ddd68f9433fb0838db119fe18c3ed) that added in all tooling (both functional and data-wise) needed to enable age verification. I reversed the surveillance enablement in this commit, and have kept all other changes since. You can see the patch file used to revert the commit here: https://github.com/Jeffrey-Sardina/systemd-suite/blob/main/main.patch

Since age collection is not needed for any aspect of `systemd`, this does not affect other aspects of `systemd`. Any downstream systems that attempt to call age-verification-related functions on Liberated `systemd` will therefore encounter an error. This is done by design. This is also why I have not simply created a "default age" as a lie -- it's about denying applications the ability to assume the presence of an API that enables mass surveillance.

## How is Liberated `systemd` tested?
To see how I run testing for this fork, see: https://github.com/Jeffrey-Sardina/systemd-suite. (In short, I run their CI pipeline before pushing changes.)

## Where else can I find Liberated `systemd`?
In order to allow users to avoid MicroSlop's ecosystem, this repository is made available via Gitea and Codeberg, on top of GitHub. The contents of all repositories are identical, and updated at the same time.
- github - https://github.com/Jeffrey-Sardina/systemd
- codeberg (mirror) - https://codeberg.org/Jeffrey-Sardina/systemd
- gitea (mirror) - https://gitea.com/Jeffrey-Sardina/systemd

## Have any other changes been made?
Only in meta-data files. Specifically, aside from code changes needed to liberate systemd from surveillance tooling, I have edited:
- this README (`/README.md`)
- the Code of Conduct (`docs/CODE_OF_CONDUCT.md`)
    - the section giving contacts of base `systemd` devs has been removed -- since the moderators of base `systemd` are, obviously, not a part of Liberated `systemd`.
- the Contributing page (`docs/CONTRIBUTING.md`)
    - this has been edited to explain how to contribute to Liberated `systemd`.
- the Security page (`docs/SECURITY.md`)
    - this has been edited to direct all security-related concerns to base `systemd`.
- the Citation file (`CITATION.cff`)
    - this has been edited to correctly identify this repo as Liberated `systemd`, a fork of `systemd/systemd`.

The original `systemd` readme is included below for reference.

---

![Systemd](http://brand.systemd.io/assets/page-logo.png)

System and Service Manager

[![OBS Packages Status](https://build.opensuse.org/projects/system:systemd/packages/systemd/badge.svg?type=default)](https://build.opensuse.org/project/show/system:systemd)<br/>
[![Semaphore CI 2.0 Build Status](https://the-real-systemd.semaphoreci.com/badges/systemd/branches/main.svg?style=shields)](https://the-real-systemd.semaphoreci.com/projects/systemd)<br/>
[![Coverity Scan Status](https://scan.coverity.com/projects/350/badge.svg)](https://scan.coverity.com/projects/systemd)<br/>
[![OSS-Fuzz Status](https://oss-fuzz-build-logs.storage.googleapis.com/badges/systemd.svg)](https://oss-fuzz-build-logs.storage.googleapis.com/index.html#systemd)<br/>
[![CIFuzz](https://github.com/systemd/systemd/actions/workflows/cifuzz.yml/badge.svg)](https://github.com/systemd/systemd/actions/workflows/cifuzz.yml)</br>
[![CII Best Practices](https://bestpractices.coreinfrastructure.org/projects/1369/badge)](https://bestpractices.coreinfrastructure.org/projects/1369)<br/>
[![Fossies codespell report](https://fossies.org/linux/test/systemd-main.tar.gz/codespell.svg)](https://fossies.org/linux/test/systemd-main.tar.gz/codespell.html)</br>
[![Translation status](https://translate.fedoraproject.org/widget/systemd/svg-badge.svg)](https://translate.fedoraproject.org/engage/systemd/)</br>
[![Coverage Status](https://coveralls.io/repos/github/systemd/systemd/badge.svg?branch=main)](https://coveralls.io/github/systemd/systemd?branch=main)</br>
[![Packaging status](https://repology.org/badge/tiny-repos/systemd.svg)](https://repology.org/project/systemd/versions)</br>
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/systemd/systemd/badge)](https://securityscorecards.dev/viewer/?platform=github.com&org=systemd&repo=systemd)

## Details

Most documentation is available on [systemd's web site](https://systemd.io/).

Assorted, older, general information about systemd can be found in the [systemd Wiki](https://www.freedesktop.org/wiki/Software/systemd).

Information about build requirements is provided in the [README file](README).

Consult our [NEWS file](NEWS) for information about what's new in the most recent systemd versions.

Please see the [Code Map](docs/ARCHITECTURE.md) for information about this repository's layout and content.

Please see the [Hacking guide](docs/HACKING.md) for information on how to hack on systemd and test your modifications.

Please see our [Contribution Guidelines](docs/CONTRIBUTING.md) for more information about filing GitHub Issues and posting GitHub Pull Requests.

When preparing patches for systemd, please follow our [Coding Style Guidelines](docs/CODING_STYLE.md).

If you are looking for support, please contact our [mailing list](https://lists.freedesktop.org/mailman/listinfo/systemd-devel), join our [IRC channel #systemd on libera.chat](https://web.libera.chat/#systemd) or [Matrix channel](https://matrix.to/#/#systemd-project:matrix.org)

Stable branches with backported patches are available in the [stable repo](https://github.com/systemd/systemd-stable).

We have a security bug bounty program sponsored by the [Sovereign Tech Fund](https://www.sovereigntechfund.de/) hosted on [YesWeHack](https://yeswehack.com/programs/systemd-bug-bounty-program)

Repositories with distribution packages built from git main are [available on OBS](https://software.opensuse.org//download.html?project=system%3Asystemd&package=systemd),
and also repositories with [packages built from the latest stable release](https://software.opensuse.org//download.html?project=system%3Asystemd%3Astable&package=systemd)