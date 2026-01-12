```yaml
number: 8722
title: Update to Rust 1.74 and use new clippy lints table
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-1.74
created_at: 2023-11-16T14:54:08Z
updated_at: 2023-11-16T23:23:22Z
url: https://github.com/astral-sh/ruff/pull/8722
synced_at: 2026-01-12T15:55:26Z
```

# Update to Rust 1.74 and use new clippy lints table

---

_@konstin_

Update to [Rust 1.74](https://blog.rust-lang.org/2023/11/16/Rust-1.74.0.html) and use the new clippy lints table.

The update itself introduced a new clippy lint about superfluous hashes in raw strings, which got removed.

I moved our lint config from `rustflags` to the newly stabilized [workspace.lints](https://doc.rust-lang.org/stable/cargo/reference/workspaces.html#the-lints-table). One consequence is that we have to `unsafe_code = "warn"` instead of "forbid" because the latter now actually bans unsafe code:

```
error[E0453]: allow(unsafe_code) incompatible with previous forbid
  --> crates/ruff_source_file/src/newlines.rs:62:17
   |
62 |         #[allow(unsafe_code)]
   |                 ^^^^^^^^^^^ overruled by previous forbid
   |
   = note: `forbid` lint level was set on command line
```

---

_Label `internal` added by @konstin on 2023-11-16 14:54_

---

_Comment by @github-actions[bot] on 2023-11-16 15:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@BurntSushi approved on 2023-11-16 15:19_

Nice!

---

_Renamed from "Updat to Rust 1.74 and use new clippy lints table" to "Update to Rust 1.74 and use new clippy lints table" by @konstin on 2023-11-16 15:34_

---

_@charliermarsh approved on 2023-11-16 17:38_

Dumb question: will Ruff still compile on Rust 1.71, given that that's what we have in our `Cargo.toml` as the `rust-version`? (I'm just wondering if this will cause issues if Conda or Homebrew aren't updated yet.)

---

_Merged by @charliermarsh on 2023-11-16 23:12_

---

_Closed by @charliermarsh on 2023-11-16 23:12_

---

_Branch deleted on 2023-11-16 23:12_

---
