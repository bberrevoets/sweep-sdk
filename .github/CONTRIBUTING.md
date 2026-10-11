# Contributing

This repository is maintained by Bert Berrevoets. It continues the archived
[scanse/sweep-sdk](https://github.com/scanse/sweep-sdk); the Scanse issue tracker, support site and community forum
no longer exist, so bug reports and feature requests go to this repository's
[issues](https://github.com/bberrevoets/sweep-sdk/issues).

## Reporting a bug

1. Search the [issues](https://github.com/bberrevoets/sweep-sdk/issues) for a similar report.
2. Name the libsweep version and the binding (libsweep, SweepPy, SweepJs or JSweep), the Sweep firmware version, the
   operating system and the hardware setup.
3. Describe what happened and what you expected to happen.
4. Add a small code snippet or test case that reproduces it, and a screenshot when that makes it clearer.

## Changing code

1. Work on a branch from `main` and open a pull request to `main` (GitHub Flow; `main` is protected and pull requests
   are squash-merged).
2. Follow the format of the existing code; libsweep and SweepJs include a `.clang-format` file.
3. Keep the libsweep C ABI stable: opaque `_s` handles and plain C types only. The bindings depend on it.
4. Test against the dummy library (`cmake .. -DDUMMY=On`) and, for changes that touch the device protocol, with a real
   Sweep. There are no automated tests.
5. Markdown must pass `npx markdownlint-cli2 "**/*.md"`.

Build commands and the known traps of this 2017–2019 code base are in [AGENTS.md](../AGENTS.md).
