# wiradesk-core Security Policy
<!-- Copied from the Wira Delta Indonesia legal source (wira-desk-core/security.en.md) on 2026-11-16.
     Edit the source, then copy it here again. -->

This is an English translation of the Indonesian original ([SECURITY.id.md](SECURITY.id.md)). If
the two differ in interpretation, the Indonesian text prevails.

**In effect since:** 2026-11-16 · applies to wiradesk-core 0.1.1 and later

wiradesk-core is a Rust library. It opens no files, makes no network requests, starts no threads,
reads no clock, and holds no operating-system handles. Every input arrives as a function argument
from the app that embeds it, and every result goes back as a return value.

What it decides from that input is where the risk is. An app can pass it key and mouse events taken
from a system-wide input hook, and decide from its answer (`Disposition`) whether to swallow them.
Wira Desk for Windows also passes it the update descriptor it downloaded, and relies on its verdict
about which installer URL to accept. Those two paths deserve scrutiny.

## Two Facts Most People Want Up Front

- **No keystroke is recorded.** The only key state the crate keeps is `ModifierState`: four
  booleans saying whether Win, Ctrl, Alt, and Shift are held. The crate has no logging, file, or
  network call anywhere in `src/`, so a key value has nowhere to go. `InputSink` is a trait; each
  app implements it, so what an implementation keeps is that app's code and that app's policy.
- **Nothing is sent.** The crate makes no outbound request. The update check in Wira Desk for
  Windows is made by the app; this crate only parses the reply the app hands to
  `update::decide`.

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
- input that makes the crate panic, overflow, or never return. The apps call it while handling
  every key event on the desktop, so a panic there can stop shortcuts from working.

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

Releases are git tags, and there are no binaries. Apps fetch the source by tag through Cargo, and
each app's `Cargo.lock` records the exact commit the tag resolved to. A build from an existing
lockfile therefore keeps using that commit even if a tag were moved; only `cargo update` would pick
up the change, and it shows up as a lockfile diff.

Tags and commits are not signed. A tag proves only that someone with push access to this repository
created it, not who. The whole crate is source code, so the strongest verification available is
reading it at the commit your `Cargo.lock` records. Signing tags is the real fix, and it is not in
place yet.

## Language

This is an English translation of the Indonesian original. If the two differ in interpretation,
the Indonesian text prevails.
