# Contributing to wiradesk-core

Contributions are welcome via pull request. This crate is kept small on purpose: it holds the
logic both Wira Desk apps run identically, and nothing else.

## Scope

Code enters this crate only when both apps need it and it behaves identically on both. Anything
tied to one operating system, the user interface, configuration files, or inter-process messages
stays in the app that owns it.

The crate does no I/O. It opens no files, makes no network requests, starts no threads, reads no
clock, and holds no operating-system handles. `#![forbid(unsafe_code)]` is set at the crate root.
If a change needs any of those, the app does that part and passes the result in as an argument.

For a new module, or a change to a public type or function, open an issue before writing code.
Apps pin this crate by tag, so a public API change reaches both of them and has to be planned
with both. A typo, a broken doc link, or a fix with a failing test can go straight to a pull
request.

## Before submitting

```text
cargo fmt
cargo clippy --all-targets -- -D warnings
cargo test
```

CI runs the same three commands on Linux when a pull request is marked ready for review.

Add a line under `Unreleased` in [`CHANGELOG.md`](CHANGELOG.md) for any change an app would
notice. Write it for the developer deciding whether to move their pin.

## Reporting bugs

- Search existing issues, open and closed, first.
- Give the tag or commit you use, the function you called, and the smallest input that shows the
  problem. A failing test is the most useful form.
- **Security issues:** do not open a public issue. Follow [`SECURITY.md`](SECURITY.md).

## Licensing and sign-off

This crate is MIT so it can be used in projects under any licence, including proprietary ones.
Your contribution is licensed under MIT as well. There is no Contributor License Agreement, and
you keep the copyright in what you write.

Code submitted here must be your own work, or come from a source under an MIT-compatible
permissive licence with that source's notice kept. It must not be copied or adapted from
GPL-licensed code, including the GPL-licensed
[Wira Desk for Windows](https://github.com/wiradeltaid/wira-desk) repository. Moving code from
there into this crate relicenses it, and only the maintainers can make that move.

### Signing off

Contributions are taken under the Developer Certificate of Origin (DCO) 1.1. Sign every commit
with `git commit -s`. Git builds the trailer from your `user.name` and `user.email`:

```text
Signed-off-by: Your Name <you@example.com>
```

Use a name you are known by and an email that reaches you; anonymous sign-offs cannot be
accepted. The email should match the commit author.

If you forgot, `git commit --amend -s` fixes the last commit and
`git rebase --signoff main` fixes every commit on your branch; then update the pull request
with `git push --force-with-lease`.

The sign-off certifies the DCO 1.1, reproduced verbatim in [`DCO`](DCO) and published at
<https://developercertificate.org>.

## Commit messages

Use [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`,
`refactor:`, `test:`). Keep the first line under 72 characters, in the imperative ("add
thirds planner", not "added thirds planner").

## Names

A contribution does not grant any right to the names "Wira Desk" or "Wira Delta Indonesia". See
"Name and trademark" in [`README.md`](README.md).

## Releases

Maintainers release by tag, and every tag after `v0.1.1` is signed. Move the `Unreleased` entries in
`CHANGELOG.md` under the new version, bump `version` in `Cargo.toml`, then tag with
`git tag -s vX.Y.Z -m "vX.Y.Z: <one line>"` and push the tag. A signing key is set per clone:
`git config gpg.format ssh`, `git config user.signingkey <path to your .pub>`,
`git config tag.gpgSign true`, and the key is registered on GitHub as a signing key. A tag is never
moved or reused; a mistake is fixed by the next tag.
