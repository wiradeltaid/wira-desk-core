# Changelog

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versioning is
[semantic](https://semver.org/). Below `1.0` the **minor** digit carries the breaking change:
`0.1.x` to `0.2.0` is the incompatible step.

Apps pin this crate by git tag, so every tag `vX.Y.Z` has a matching `## [X.Y.Z]` section here.
Write each entry for the developer deciding whether to move their pin, not for the commit log.
Work that needs a minor or major bump stays under **Unreleased** until the maintainers decide to
release it.

## [Unreleased]

## [0.1.1] - 2026-11-16

### Fixed

- `update` accepted installers only from this crate's own repository after the split, so the Windows
  app's updater refused every real release. `update::REPOSITORY` again names
  `https://github.com/wiradeltaid/wira-desk`, where Windows releases are published. Move pins from
  `v0.1.0` to `v0.1.1`; `v0.1.0` should not be used.

## [0.1.0] - 2026-11-16

### Added

- Snapping geometry (`arrangement`): halves, thirds, maximize, custom-percentage edge snaps,
  overlapping stacks of up to three windows, and moving a window to the next monitor. Rectangles
  use signed physical pixels with half-open edges, and every arithmetic step is checked, so an
  unrepresentable geometry returns `PlanError` instead of a wrapped placement.
- Same-app window cycling (`cycling`): application identity from the executable path, window
  eligibility from facts the app supplies, and stateless next-window selection.
- Context policy (`context`): whether a shortcut applies on the current monitor and foreground
  window, or passes through.
- Visual switcher models (`switcher`): adaptive tile layout with height-bounded row packing, and
  card selection across rows and pages.
- Command and effect model (`commands`, `input`): the `Command` set, `ModifierState`, and the
  `InputSink` trait that turns an input event into a `Disposition` and an `Effect` without calling
  the operating system.
- Update decision (`update`): parses a release descriptor, compares versions, and accepts only an
  HTTPS download under the release downloads of the `repository` named in this crate's
  `Cargo.toml`, with a well-formed SHA-256.
