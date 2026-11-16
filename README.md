# wiradesk-core

The shared domain logic of Wira Desk: window snapping geometry, same-app window cycling, and the
command and effect model the apps act on. It has no operating-system code and does no I/O, so every
part of it runs and is tested the same way on any platform.

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
cargo test
cargo clippy --all-targets -- -D warnings
```

## Licence

MIT. See [LICENSE](LICENSE).
