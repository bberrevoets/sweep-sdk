# AGENTS.md

Instructions for AI coding agents (Claude Code, Codex and others) working in this repository. `CLAUDE.md` only imports
this file, so edit this one. [README.md](README.md) describes the project for people; this file lists what the code
alone does not tell you.

## Project Overview

SDK for the Scanse Sweep, a 360° 2D LiDAR that talks over a USB serial port (FTDI). `libsweep` is a C99 library with
an optional C++11 header; SweepPy (Python, ctypes), SweepJs (Node.js native addon) and JSweep (Java, JNA) wrap it.

This is Bert's own repository, started as a fork of [scanse/sweep-sdk](https://github.com/scanse/sweep-sdk) and
detached from that fork network on 2026-10-11 so he can develop it further. Upstream is **archived** (last commit
2019-08-05: SDK v1.3.0, device firmware v1.4) and Scanse's support site and forum are gone; there is nothing to sync
from. Up to the detach, the code is upstream's unchanged. *Guess, to confirm with Bert:* it belongs with his ROS
experiments with a Sweep sensor (see Cross-Repository).

## Commands

libsweep must be built and installed first; every binding loads the installed library.

```sh
cd libsweep && mkdir -p build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release            # add -DDUMMY=On for a fake device with a static point cloud
cmake --build .
sudo cmake --build . --target install && sudo ldconfig
```

- **Windows:** `cmake .. -G "Visual Studio 17 2022" -A x64`, then `cmake --build . --config Release` and the `install`
  target from an administrator prompt (installs to `C:\Program Files\sweep`). The README's `"Visual Studio 14 2015 Win64"`
  form needs Visual Studio 2015; current generators take the platform through `-A`.
- **Examples:** `libsweep/examples` is a separate CMake project (`find_package(Sweep)`); the viewer needs SFML, the
  network example protobuf + ZeroMQ.
- **SweepPy:** install with `pip install ./sweeppy`, then `python -m sweeppy <port>` works from any folder. Without
  installing, run it from the package folder: `cd sweeppy && python -m sweeppy <port>` — from the repository root
  Python takes the outer `sweeppy/` folder for a namespace package and fails with `No module named sweeppy.__main__`.
  Either way the package loads the installed libsweep at import.
- **SweepJs:** `npm install` in `sweepjs` (node-gyp build, `nan` ^2.2); `npm test` opens `/dev/ttyUSB0`.
- **JSweep:** `./gradlew build` in `jsweep` (Gradle wrapper 3.5), **Linux only**. Its `buildLibSweep` task
  ([jsweep/libsweep-bundling.gradle](jsweep/libsweep-bundling.gradle)) deletes `libsweep/build`, configures it again
  with `-DDUMMY=Off` and bundles `libsweep.so` into a `linux-x86-64` natives jar. Pass `-PlibsweepDummy=true` to build
  against the dummy library, and expect any manual build in `libsweep/build` to be wiped.
- **Docker:** `docker build -t sweep . && docker run -e GRADLE_TASK=build sweep` runs `docker/build.sh` in a
  manylinux1 (CentOS 5) image: a dummy libsweep build, then `./gradlew build`, which rebuilds libsweep without
  `DUMMY` (see JSweep). *Untested since 2019*; the base image is long out of support.
- **Tests:** there are none, and there is no CI: the upstream Travis and AppVeyor setup was removed on 2026-10-11 (it
  only built against the dummy library). A real device is the only test ([RELEASE.md](RELEASE.md)).
- **Markdown lint:** `npx markdownlint-cli2 "**/*.md"`.

## Repository Layout

```text
libsweep/            C library: include/sweep/sweep.h (+ sweep.hpp), src/ (protocol, serial unix/win, dummy, sweep-ctl)
libsweep/doc/        serial_protocol_spec.md — the device's serial protocol
libsweep/examples/   C and C++ examples, SFML viewer, protobuf/ZeroMQ network example
libsweep/man/        sweep-ctl manual page
sweeppy/             Python bindings (ctypes), python -m sweeppy runs a demo
sweepjs/             Node.js addon (binding.gyp, sweepjs.cc) and a websocket example
jsweep/              Java bindings (JNA), Gradle build that bundles the native library
Dockerfile, docker/  manylinux1 image that builds libsweep (dummy) and jsweep
.github/             contributing guide, issue and pull request templates
RELEASE.md           release checklist (version numbers live in four files)
```

## Conventions

- **Stable C ABI:** the public API uses only opaque `_s` handles and plain fixed-size C types; every object is
  `_construct()`ed and `_destruct()`ed explicitly. Keep that when changing `sweep.h` — the bindings depend on it.
- **Versions in four places:** `libsweep/CMakeLists.txt`, `sweeppy/setup.py`, `sweepjs/package.json` and
  `jsweep/gradle.properties` must carry the same version.
- **Units:** angles in milli-degrees, distances in centimetres, signal strength 0–255; motor speed 0–10 Hz, sample rate
  500 / 750 / 1000 Hz.
- **Old toolchains (2017–2019 code):**
  - CMake 4.x refuses `cmake_minimum_required(VERSION 2.8.11)`; add `-DCMAKE_POLICY_VERSION_MINIMUM=3.5`.
  - Gradle 3.5 runs only on JDK 7/8, and `build.gradle` uses `jcenter()` and the `compile` configuration, both gone in
    current Gradle.
  - SweepJs was last built on Node 4/6.
  - Expect fixes before any of the bindings build on a current machine.
- **`jsweep/gradle/wrapper/gradle-wrapper.jar`** stays a plain git blob, not Git LFS: the wrapper must work in a clone
  without LFS.
- **Windows serial ports:** Sweep needs non-default FTDI driver settings, once per COM port (Device Manager → port →
  Port Settings → Advanced): USB Transfer Sizes **64** bytes for Receive and Transmit, latency timer (BM Options)
  **2** ms. Without them data does not arrive correctly (Scanse's guide, archived copy linked in
  [libsweep/README.md](libsweep/README.md)).
- **Dead links:** `support.scanse.io` and `community.scanse.io` no longer exist. The READMEs link Wayback Machine
  copies where one exists (the Best Practices article has none); the user manual on `s3.amazonaws.com/scanse` was
  still online on 2026-10-11.
- **Two lint exceptions, on purpose:** `.github/.markdownlint.json` extends the root config and only turns off MD041
  (an H1 would land in every issue and PR body), and `libsweep/man/sweep-ctl.md` disables MD041 in a comment at its
  end because pandoc needs the `%` title block on line 1 (`libsweep/man/build.sh` turns it into `sweep-ctl.1`).

## Cross-Repository

- [bberrevoets/sweep-ros](https://github.com/bberrevoets/sweep-ros) — the ROS 1 driver and node, which links against
  libsweep from this repository (install it first). Bert's own private repository since 2026-10-11 (detached from
  `scanse/sweep-ros`); cloned next to this one in `D:\Repos\HardwareProjects\Sweep\sweep-ros`.
- [bberrevoets/l3xz_sweep_scanner](https://github.com/bberrevoets/l3xz_sweep_scanner) — another ROS driver for the
  Sweep (fork of `107-systems/l3xz_sweep_scanner`), still under review on 2026-10-11.

<!-- repo-init:start -->
## Repository Info

- **Company:** Berrevoets Systems
- **Author:** Bert Berrevoets
- **Contact:** <bert@berrevoets.net>
- **Remote:** <https://github.com/bberrevoets/sweep-sdk> (public)

## Workflow Rules

- **GitHub Flow:** `main` is the default and only long-lived branch. It is protected by the ruleset "Protect main":
  no direct pushes, no deletion, changes only through a pull request, and all PR conversations must be resolved
  before merging. Merged head branches are deleted automatically.
- After cloning, run `git config pull.rebase true` once, so `git pull` rebases instead of creating merge commits.
- Work on a branch per topic, push it, and open a PR to `main` only when Bert asks for it, assigned to him
  (`gh pr create --assignee bberrevoets`). PRs are squash-merged. After a merge: switch to `main` and pull, then delete the
  local branch with `git branch -D <branch>` (`-d` refuses after a squash merge) once
  `gh pr view <branch> --json state,headRefOid` shows `MERGED` and the same commit as `git rev-parse <branch>`.
- **"Update the documentation"** means `AGENTS.md`, all `README.md` files and `CHANGELOG.md`; then commit and push.
- [CHANGELOG.md](CHANGELOG.md) follows Keep a Changelog: the Attributors list at the top, and one `Author: *Name*`
  line under each section heading (`### Added`, `### Changed`, ...).
- Markdown must pass `markdownlint-cli2 "**/*.md"` with [.markdownlint.json](.markdownlint.json) (180-character
  lines) before every commit.
- Binaries go through Git LFS: every `filter=lfs` pattern in `.gitattributes` — the standard set (PDF, ZIP, PNG,
  JPEG, STEP/STP, STL, 3MF, F3D/F3Z) plus any pattern this repository adds. Add a pattern there before committing
  another binary type (GIF, BMP, ...). The patterns are lowercase and match `X.PNG` only where `core.ignorecase` is
  true (Windows, macOS); on a case-sensitive system check a new binary with `git check-attr filter -- <file>` before
  committing it.
- Commit messages use the conventional style (`feat:`, `fix:`, `docs:`, `chore:`).
<!-- repo-init:end -->
