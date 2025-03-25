# Ubuntu Excuses

Once a package is uploaded to the Ubuntu archive (dput) it triggers a series of tests and rebuilds across multiple packages in the archive.
Said package won't be able to migrate until it has build properly and all its dependencies have had successful tests

This is a very simple view but this is what we call package migration.

When a package doesn't migrate, one would wonder why? what is its excuse?

## introducing ubuntu-excuses

While visual-excuses allows to see the interactions between blocked packages in the proposed pocket, this tool offers a command line
le This tool also uses the package per team mapping relevant to Canonical's internal teams which could help to show excuses per team. The content comes from http://reqorts.qa.ubuntu.com/reports/m-r-package-team-mapping.html


## Installation

ubuntu-excuses is available as a snap and installing it should be as simple as
```
$> snap install ubuntu-excuses
```

Otherwise it can also be installer as a standard pip python package

```
$> git clone https://github.com/mclemenceau/ubuntu-excuses.git
$> cd ubuntu-excuses
$> pip3 install .
```

## Usage
By default if used without argument, ubuntu-excuses will show you the list of all the excuses.
However multiple command line flags can be used to filter the relevant excuses

``` bash
$> ubuntu-excuses --help
usage: ubuntu-excuses [-h] [--inspect INSPECT] [--name NAME] [--component COMPONENT] [--team TEAM] [--ftbfs] [--min-age MIN_AGE] [--max-age MAX_AGE] [--limit LIMIT] [--reverse] [--json]

Ubuntu Excuses (Proposed Migration) Viewer

options:
  -h, --help            show this help message and exit
  --inspect INSPECT     Get details about a specific package
  --name NAME           Regex to filter package names (case-insensitive)
  --component COMPONENT
                        Archive component (main, restricted, universe, multivere)
  --team TEAM           Show only packages subscribed by this Ubuntu team
  --ftbfs               Show only FTBFS packages
  --min-age MIN_AGE     Only include packages at least this many days old
  --max-age MAX_AGE     Only include packages no older than this many days
  --limit LIMIT         Limit the number of results shown
  --reverse             Show excuses from older to more recent
  --json                Output in JSON format
```

For example to list all the excuses affecting a specific distro team, here foundations you can simply run

``` bash
$> ubuntu-excuses --team foundations-bugs
╒════════╤══════════════════════╤═════════════╤════════════════════════╤═════════╤══════════════╕
│   Days │ Package              │ Component   │ New Version            │ FTBFS   │ Excuse Bug   │
╞════════╪══════════════════════╪═════════════╪════════════════════════╪═════════╪══════════════╡
│      0 │ ubuntu-x1e-settings  │ main        │ 25.04.6                │         │              │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│      1 │ libidn               │ main        │ 1.43-1                 │         │              │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│      1 │ libidn2              │ main        │ 2.3.8-2                │         │              │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│      2 │ llvm-toolchain-20    │ main        │ 1:20.1.1-1~exp1ubuntu1 │ ✅      │ LP: #2103459 │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│      4 │ cross-toolchain-base │ main        │ 74ubuntu1              │ ✅      │              │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│      8 │ expat                │ main        │ 2.7.0-1                │         │ LP: #2103943 │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│     32 │ build-essential      │ main        │ 12.12                  │         │ LP: #2101890 │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│     32 │ bpftrace             │ main        │ 0.21.2-2ubuntu4        │ ✅      │ LP: #2099792 │
├────────┼──────────────────────┼─────────────┼────────────────────────┼─────────┼──────────────┤
│     46 │ libsepol             │ main        │ 3.8-1                  │         │ LP: #2099944 │
╘════════╧══════════════════════╧═════════════╧════════════════════════╧═════════╧══════════════╛

```

In this other example, you can filter all the ftbfs affecting rust packages from older to more recent

``` bash
$> ubuntu-excuses --ftbfs --name rust --reverse
╒════════╤══════════════════════════╤═════════════╤════════════════╤═════════╤══════════════╕
│   Days │ Package                  │ Component   │ New Version    │ FTBFS   │ Excuse Bug   │
╞════════╪══════════════════════════╪═════════════╪════════════════╪═════════╪══════════════╡
│     45 │ rust-safe-arch           │ universe    │ 0.7.4-2        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│     12 │ rust-debian-repro-status │ universe    │ 0.2.1-1ubuntu1 │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│     10 │ rust-forensic-adb        │ universe    │ 0.8.0-1        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-vivid               │ universe    │ 0.9.0-3        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-syntect-no-panic    │ universe    │ 6.0.0-2        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-syntect             │ universe    │ 5.2.0-2        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-lsd                 │ universe    │ 1.1.5-4        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-erbium-core         │ universe    │ 1.0.5-6        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      4 │ rust-config              │ universe    │ 0.15.9-2       │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      3 │ rust-spytrap-adb         │ universe    │ 0.3.4-1        │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      2 │ arm-trusted-firmware     │ universe    │ 2.12.1+dfsg-1  │ ✅      │              │
├────────┼──────────────────────────┼─────────────┼────────────────┼─────────┼──────────────┤
│      1 │ rust-rustls              │ universe    │ 0.23.25+ds-1   │ ✅      │              │
╘════════╧══════════════════════════╧═════════════╧════════════════╧═════════╧══════════════╛
```

## How to Contribute
Oddly this package is only used to build the snap, but if you want to add new 
command line parameter or new functionaities, head over to
https://github.com/mclemenceaa/visual-excuses and start a discussion by creating
an issue https://github.com/mclemenceaa/visual-excuses/issues
