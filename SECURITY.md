# wiradesk-core Security Policy
<!-- Copied from the Wira Delta Indonesia legal source (wira-desk-core/security.en.md) on 2026-11-16.
     Edit the source, then copy it here again. -->

This is an English translation of the [Indonesian
original](https://github.com/wiradeltaid/wira-desk-core/blob/main/SECURITY.id.md). If the two differ in
interpretation, the Indonesian text prevails.

**In effect since:** 2026-11-16 · applies to wiradesk-core 0.1.1 and later

wiradesk-core is a sans-IO Rust library. Its code does not open files, make network requests, start
threads, read a clock, or call operating-system APIs. Apps pass data as arguments; functions can return
values or change data passed through mutable arguments.

The risk depends on how an app uses the library's decisions and types. An app can pass key and mouse
events from a system-wide input hook to its `InputSink` implementation, then use `Disposition` to decide
whether to pass an event onward. Wira Desk for Windows also passes its downloaded update descriptor to
`update::decide` to determine whether to offer a release.

## Two Facts Most People Want Up Front

- **The crate does not record keystrokes.** `ModifierState` stores four booleans for the state of Win,
  Ctrl, Alt, and Shift. `InputEvent::Key` carries a key value to `InputSink`, but the crate has no
  logging, file, or network calls in `src/`. Each app implements `InputSink` and is responsible for how
  its implementation handles or stores events.
- **The crate sends no network requests.** Wira Desk for Windows performs the update check in the app;
  the crate parses and checks the descriptor the app passes to `update::decide`.

`#![forbid(unsafe_code)]` is set at the crate root, so the crate contains no `unsafe` code and the
compiler refuses any.

## Reporting a Vulnerability

Use **GitHub Security Advisories** on this repository ("Report a vulnerability" on the Security
tab), so that the report stays private until a fix exists. If you cannot use GitHub, email
[security@wiradelta.com](mailto:security@wiradelta.com). Do not open a public issue for a suspected
vulnerability.

Helpful in a report: the tag or commit, the function you called, the input, what happened, and, if
you have one, a failing test. There is no bounty program and no guaranteed response time. This is a
small project, and honesty about that is more useful than a promise it cannot keep.

**In scope:**

- a descriptor that makes `update::decide` offer a release it should refuse: a download URL outside
  the release downloads of the Windows app's repository (`github.com/wiradeltaid/wira-desk`), a URL
  that is not HTTPS, a checksum that is not 64 hexadecimal characters, or a version that is not
  newer than the running one;
- input that makes the crate panic, overflow, or never return. A failure while an app handles input
  can disrupt shortcuts.

**Out of scope, documented rather than fixed:**

- how an app collects input or acts on a result: hooks, window handles, permissions, downloads, and
  installers are app code. Report those through that app's own security policy;
- whether the downloaded installer matches `setup_sha256`. `update::decide` checks that the
  checksum has the right form, not the file, because the crate never sees the file. Comparing the
  two is the app's job, and a missing comparison is an app bug;
- vulnerabilities in `serde` or `serde_json` themselves. They are separate projects that publish
  their own fixes, so report those to their maintainers.

**Supported versions.** Only the latest tag receives fixes. There is no long-term support branch.

## Release Integrity

Releases are Git tags, with no binaries. When an app obtains a Git dependency through Cargo,
`Cargo.lock` records the resolved commit. While that lockfile entry remains in use, builds use that
commit even if the tag moves. Updating dependencies, changing the manifest, or regenerating the lockfile
can produce a new resolution; check the `Cargo.lock` diff for a changed commit.

Tags `v0.1.0` and `v0.1.1` are unsigned, so they provide no cryptographic proof of a signer. Every tag
after `v0.1.1` is signed with an SSH key registered to the maintainer's GitHub account. GitHub can
display a Verified status; to check a tag locally with `git tag -v`, configure
`gpg.ssh.allowedSignersFile` with the signer's public key after establishing that you trust it. Commits
are unsigned. You can also review the code at the commit recorded in your `Cargo.lock`.

## Language

This is an English translation of the Indonesian original. If the two differ in interpretation,
the Indonesian text prevails.
