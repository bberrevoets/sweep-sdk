# Changelog

All notable changes to this repository are documented in this file. From [Unreleased] on, the format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/); the sections from v1.3.0 down are the upstream changelog of
[scanse/sweep-sdk](https://github.com/scanse/sweep-sdk) as it was when upstream was archived.

## Attributors

- Bert Berrevoets (<bert@berrevoets.net>) — Berrevoets Systems — maintainer since 2026
- Claude Opus 5.5 — co-author of the AI agent setup
- Daniel J. Hofmann, Scanse LLC and the upstream contributors — the SDK up to v1.3.0

## [Unreleased]

### Added

Author: *Bert Berrevoets*

- AI agent setup: [AGENTS.md](AGENTS.md) as the single instruction file, a [CLAUDE.md](CLAUDE.md) stub that imports it,
  markdownlint (180 characters), Git LFS rules and the Attributors in this changelog.

### Changed

Author: *Bert Berrevoets*

- Repository detached from the archived scanse/sweep-sdk fork network; default branch renamed from `master` to
  `main` (GitHub Flow); the upstream side branches (`cleanup-dummy`, `docker`, a dependabot branch) removed.
- README: an Authors section that credits the upstream authors and names the maintainer.
- `.github` contributing guide and issue / pull request templates rewritten for this repository (the Scanse issue
  tracker and forum they pointed to are gone).
- All inherited Markdown files reformatted to pass markdownlint (headings, tables, line length, code fences), as a
  formatting-only change. The protocol spec's command sections now share one heading level.
- `RELEASE.md` points to this repository's releases and matches the protected-`main` workflow.
- Dead `support.scanse.io` links replaced by Wayback Machine copies (the Best Practices article has none).
- SweepPy README: links are absolute links to this repository, because `setup.py` also publishes the README as the
  package's long description, where relative links break.
- `RELEASE.md` and AGENTS.md: JSweep builds against the dummy library only with `-PlibsweepDummy=true`.

### Removed

Author: *Bert Berrevoets*

- Upstream CI that no longer runs: `.travis.yml`, `appveyor.yml`, the `travis/` scripts and the README badges.

## v1.3.0

This release is compatible with device firmware v1.4.

- libsweep:
  - Added FreeBSD support, thanks to [Hyun Hwang](https://github.com/hyunh90)
- jsweep:
  - Added new Java bindings `jsweep`, thanks to [Kenzie Togami](https://github.com/kenzierocks)

## v1.2.3

This release is compatible with device firmware v1.4.

Changes:

- libsweep:
  - Fixed bug in serial impl. on Linux causing device creation to hang on some systems

## v1.2.2

This release is compatible with device firmware v1.4.

Changes:

- libsweep:
  - Fixed a bug causing sweep object to hang during device creation.
  - Refactor libsweep internals

## v1.2.1

This release is compatible with device firmware v1.4.

Changes:

- libsweep:
  - Fixed c++ interface, to resolve linker errors when header is included in multiple translation units.

## v1.2.0

This release is compatible with device firmware v1.4.

Changes:

- libsweep:
  - Mac OS support
  - Propagate dynamic type of exception from background thread
- sweeppy
  - Added patch version (ie: MAJOR.MINOR.PATCH)

## v1.1.2

This release is compatible with device firmware v1.4.

Changes:

- libsweep:
  - Adapts device construction to be compatible with RaspberryPi
  - Removes implementation methods from API
  - Propagates errors from background thread when accumulating scans

## v1.1.1

This release is compatible with device firmware v1.2.

Changes:

- libsweep: Adapts motor calibration timeout to firmware changes

## v1.1.0

This release is compatible with device firmware v1.1.

Changes:

- libsweep: Adaptes protocol implementation to firmware changes
- libsweep: Accumulates scans in a background worker thread

## v0.1.0

This is the initial release v0.1.0 and is compatible with the Sweep device firmware version v1.0.

It comes with C99, C++11, Python2 / Python3, and Node.js bindings.

Some examples we include:

    Real-time point cloud viewer
    Pub-Sub networking example
    Websocket daemon for device

Happy scanning!
