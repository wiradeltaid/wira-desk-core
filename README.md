# wiradesk-core

The shared domain logic of Wira Desk: window snapping geometry, same-app window cycling, and the
command and effect model the apps act on. It has no operating-system code and does no I/O, so every
part of it runs and is tested the same way on any platform.

[Website](https://wiradelta.com/wira-desk/) | [Changelog](CHANGELOG.md) | [Contributing](CONTRIBUTING.md) | [License](LICENSE) | [Security](SECURITY.md)

---

Two apps build on it:

- [Wira Desk](https://github.com/wiradeltaid/wira-desk) for Windows, GPL-3.0.
- Wira Desk for macOS, a commercial app.

## What belongs here

Code moves into this crate only when both apps need it and it behaves identically on both. Anything
tied to one operating system, the user interface, configuration files, or inter-process messages
stays in the app that owns it.

## Using it

```toml
wiradesk-core = { git = "https://github.com/wiradeltaid/wira-desk-core.git", tag = "v0.1.0" }
```

To work on the core and an app together, point the app at a local checkout:

```toml
[patch."https://github.com/wiradeltaid/wira-desk-core.git"]
wiradesk-core = { path = "../wira-desk-core" }
```

## Testing

```text
cargo fmt --check
cargo clippy --all-targets -- -D warnings
cargo test
```

## Contributing

Pull requests are welcome. [CONTRIBUTING.md](CONTRIBUTING.md) explains the scope rule above, the
MIT-only rule for incoming code, and the DCO sign-off. Changes are listed in
[CHANGELOG.md](CHANGELOG.md).

## Licence and attribution

- MIT, copyright Wira Delta Indonesia. See [LICENSE](LICENSE).
- [SECURITY.md](SECURITY.md): how to report a vulnerability privately, and which problems are in
  scope.
- This crate does no I/O and sends nothing over the network, so it has no privacy policy of its
  own. Each app that builds on it publishes one.
- Third-party crates: [serde](https://crates.io/crates/serde) and
  [serde_json](https://crates.io/crates/serde_json), both MIT OR Apache-2.0. Cargo fetches them at
  build time; this repository contains none of their code.

## Name and trademark

The licence in `LICENSE` covers the code in this repository. It does not cover the names
"Wira Desk" and "Wira Delta Indonesia", or their logos and wordmarks.

You may use these names to refer to the original project: "based on wiradesk-core", "a fork of
wiradesk-core", "compatible with Wira Desk". You may not use them as the name of your own product,
or in a way that suggests you are this project or are endorsed by it.

For forks: please rename. The code is yours; the name is not.
