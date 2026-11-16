---
name: Bug report
about: Report a wrong result or a panic in wiradesk-core
title: '[BUG] '
labels: bug
assignees: ''
---

<!-- Security issues do not belong here. See SECURITY.md. -->

**Describe the bug**
What the crate returned, and what you expected it to return.

**To reproduce**
The function you called and the input you gave it. A failing test is the best form:

```rust
#[test]
fn reproduces_the_bug() {
    // ...
}
```

**Environment**
- wiradesk-core tag or commit:
- `rustc --version`:
- Target (for example `x86_64-pc-windows-msvc`, `aarch64-apple-darwin`):
- Found through an app? Which one, and which version:

**Additional context**
Anything else that helps.
