```yaml
number: 17123
title: "fix: add ad-hoc code signing for Python binaries on macOS"
type: pull_request
state: closed
author: majiayu000
labels: []
assignees: []
base: main
head: fix/macos-python-codesign
created_at: 2025-12-13T10:22:39Z
updated_at: 2025-12-13T16:39:29Z
url: https://github.com/astral-sh/uv/pull/17123
synced_at: 2026-01-10T05:49:14Z
```

# fix: add ad-hoc code signing for Python binaries on macOS

---

_Pull request opened by @majiayu000 on 2025-12-13 10:22_

## Summary

On macOS Sequoia and later, binaries with the `com.apple.provenance` extended attribute may be rejected by Gatekeeper (SIGKILL) unless they are properly signed. This adds ad-hoc code signing to Python binaries after installation to ensure they can execute without issues.

## Changes

- Added `adhoc_codesign()` function in `crates/uv-python/src/macos_dylib.rs`
- Added `MissingCodesign` and `CodesignError` error variants for better error handling
- Added `ensure_codesigned()` method in `crates/uv-python/src/managed.rs`
- Updated `installation.rs` and `commands/python/install.rs` to call `ensure_codesigned()` after installation

The fix applies `codesign --force -s -` to the Python executable after installation, which clears any cached Gatekeeper rejection and allows the binary to run.

## Test Plan

- [x] `cargo test --package uv-python` - all 112 tests pass
- [x] `cargo clippy --all` - no warnings
- [x] `cargo fmt --check` - passes

Fixes #16726
Fixes #16003

---

_Assigned to @zanieb by @zanieb on 2025-12-13 15:27_

---

_Comment by @zanieb on 2025-12-13 15:28_

I think we'll probably just sign these upstream when we distribute them with a proper key. It's plausible we should do this as an intermediate step though, thanks for the pull request.

---

_Closed by @majiayu000 on 2025-12-13 16:39_

---
