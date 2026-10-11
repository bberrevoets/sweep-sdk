# Sweep SDK

[![Continuous Integration](https://travis-ci.org/scanse/sweep-sdk.svg?branch=master)](https://travis-ci.org/scanse/sweep-sdk)
[![Continuous Integration](https://ci.appveyor.com/api/projects/status/github/scanse/sweep-sdk?svg=true)](https://ci.appveyor.com/project/kent-williams/sweep-sdk)

SDK for Scanse Sweep LiDAR.

- [libsweep](libsweep/README.md): low-level ABI/API-stable C library
- [SweepPy](sweeppy/README.md): Python bindings
- [SweepJs](sweepjs/README.md): NodeJS bindings
- [JSweep](jsweep/): Java bindings

Real-time viewer for a device speed of 5 Hz:

![viewer](https://cloud.githubusercontent.com/assets/527241/20300444/92ade432-ab1f-11e6-9d96-a585df3fe471.png)

Density-based clustering on the point cloud:

![dbscan](https://cloud.githubusercontent.com/assets/527241/20300478/b5ae968e-ab1f-11e6-8ee0-d24aedd835f9.png)

## License

Copyright © 2016 Daniel J. Hofmann

Distributed under the MIT License (MIT).

## Conventions

- **Branching:** GitHub Flow. `main` is protected and accepts changes only through pull requests.
- **Markdown:** linted with `markdownlint-cli2`, 180 character limit.

  ```sh
  npx markdownlint-cli2 "**/*.md"
  ```

- **Binaries:** tracked with Git LFS. Run `git lfs install` once per machine before cloning; in a clone made
  without it, run `git lfs install` and then `git lfs pull` to download the files.
- **Agent instructions:** in [AGENTS.md](AGENTS.md), read by Claude Code, Codex and any other coding agent.
  `CLAUDE.md` only imports it, so edit `AGENTS.md`.

## Authors

- SDK: Daniel J. Hofmann, Scanse LLC and the contributors of the archived upstream
  [scanse/sweep-sdk](https://github.com/scanse/sweep-sdk).
- This repository (detached from the upstream fork network on 2026-10-11): Bert Berrevoets — Berrevoets Systems —
  <bert@berrevoets.net>
