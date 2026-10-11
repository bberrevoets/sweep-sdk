# Publishing a new version

- [ ] Move the `[Unreleased]` entries in `CHANGELOG.md` to a new version section with today's date
- [ ] Sync. version number in `libsweep/CMakeLists.txt`, `sweeppy/setup.py`, `sweepjs/package.json`, `jsweep/gradle.properties`
- [ ] Merge these changes to `main` through a pull request (`main` is protected)
- [ ] Build libsweep and the bindings against the dummy library (`cmake .. -DDUMMY=On`; JSweep on Linux:
  `./gradlew build -PlibsweepDummy=true`, see AGENTS.md); there is no CI
- [ ] Test with a real device
- [ ] Tag the merged commit on `main`: `git tag -a vx.y.z <sha>` (we use [semantic versioning](https://semver.org/))
- [ ] Push the tag: `git push origin vx.y.z`
- [ ] Make a release on <https://github.com/bberrevoets/sweep-sdk/releases> (or `gh release create vx.y.z`)
