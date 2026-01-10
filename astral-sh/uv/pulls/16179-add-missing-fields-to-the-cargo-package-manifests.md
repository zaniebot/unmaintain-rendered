```yaml
number: 16179
title: Add missing fields to the Cargo package manifests
type: pull_request
state: merged
author: ruben-arts
labels:
  - internal
assignees: []
merged: true
base: main
head: align-crate-cargo-toml-files
created_at: 2025-10-08T08:48:56Z
updated_at: 2025-10-08T10:02:02Z
url: https://github.com/astral-sh/uv/pull/16179
synced_at: 2026-01-10T06:36:15Z
```

# Add missing fields to the Cargo package manifests

---

_Pull request opened by @ruben-arts on 2025-10-08 08:48_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Hey devs, great tool as always, you're doing amazing work. 

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds the following fields to the `[package]` table of `Cargo.toml` files where they were missing:
```toml
rust-version = { workspace = true }
homepage = { workspace = true }
documentation = { workspace = true }
repository = { workspace = true }
authors = { workspace = true }
license = { workspace = true }
```

Most crates already had these fields, this just aligns the rest for consistency.

This also resolves the warnings from `cargo-deny` when using `uv` crates as dependencies in Pixi.
## Test Plan
No tests needed, this only updates metadata.


---

_@konstin approved on 2025-10-08 10:01_

---

_Merged by @konstin on 2025-10-08 10:01_

---

_Closed by @konstin on 2025-10-08 10:01_

---

_Label `internal` added by @konstin on 2025-10-08 10:02_

---
