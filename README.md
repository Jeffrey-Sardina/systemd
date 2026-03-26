# Liberated `systemd`

Mass surveillance is bad, actually. So here's a fork of `systemd` with surveillance enablement removed, which will be kept up-to-date with other changes in `systemd/main`. However you use this, or do not, is your choice and yours alone.

## Purpose
The purpose of Liberated `systemd` is to do exactly one thing, and do it well: removing surveillance enablement from base `systemd`. Specifically, here is what I mean by surveillance: **surveillance is the tooling that enables or facilitates collection of any personal information that does not arise from technical needs for `systemd`**. The primary offender of this is, of course, age verification. If `systemd` later adds in support for other surveillance mechanisms, those will also be removed.

What this also means is that Liberated `systemd` is not a divergent development project. It will not introduce new features, correct bugs or security issues, or implement optimizations. If you want to contribute to any of those things, the correct way to do so is to raise a PR against the base `systemd/systemd` repo. This repo exists only to remove surveillance enablement.

## Distribution/users maintainers, please read

Systemd devs chose a path to centralizing the "age-verification" process.

This itself is a little part of the well-planned operation to sabotage whole OpenSource.

At first, the distributions like Ubuntu and RHEL, will implement systemd age-check integration.

The other distros like Arch, Nix etc, may follow this, for software-compatibility.

**HUGE BUT!**

This inevitable creates much easier adoption process in whole linux community.

And the governments acknowledged it.

They will inevitable start implementing "phase 2", forcing the systemd to integrate hardware based age check (like TPM?).

Unlike a soft-one, the hardware level will completely handle the internal crypto-based logic, accept the incoming device-encrypted private key for proving personal data integrity with no way for user to escape forger it.

This sabotages the whole computing, because after implementing all of that, the software will know not only your age, but obviously:
- Unique identity key associated with you
- Your country
- Your passpord ID
- etc.

And will inevitable use it for survelliance, geo-restrictions, geo-based attacks and et cetera.

The worst of that, is that systemd decided to assist in implementing this miserable future for all of us.

We understand, that many distributions are dependent on systemd and cannot simply move out of it.

We suggest such distribution users & maintainers to:
- Use this fork instead of systemd by default
- Assist in this fork development
- Unity and communicate for future action coordination
- Protest against any form of age-verification

## How often is this updated (or, "why is Liberated `systemd` behind by X commits?")
Liberated `systemd` will be updated at least weekly. I hope to automate this process soon, at which point updates will be near real-time. (If you want to help automate this, please feel free to raise a PR against  https://github.com/Jeffrey-Sardina/systemd-suite, which is where I am implementing testing and update automation).

## How is Liberated `systemd` implemented?
It's quite simple: `systemd`, very nicely, has atomic commits. There is exactly one commit (https://github.com/systemd/systemd/commit/acb6624fa19ddd68f9433fb0838db119fe18c3ed) that added in all tooling (both functional and data-wise) needed to enable age verification. I reversed this commit, and have kept all other changes since.

Since age collection is not needed for any aspect of `systemd`, this does not affect other aspects of `systemd`. Any downstream systems that attempt to call age-verifiaction-related functions on Liberated `systemd` will therefore encounter an error. This is done bey design. This is also why I have not simply created a "default age" as a lie -- it's about denying applications the ability to assume the presence of an API that enables mass surveillance.

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
