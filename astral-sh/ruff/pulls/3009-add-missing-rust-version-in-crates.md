```yaml
number: 3009
title: Add missing rust-version in crates
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: add-missing-rust-version
created_at: 2023-02-18T13:23:41Z
updated_at: 2023-02-23T19:55:27Z
url: https://github.com/astral-sh/ruff/pull/3009
synced_at: 2026-01-12T15:55:12Z
```

# Add missing rust-version in crates

---

_@JonathanPlasse_

I used `cargo-msrv` to determine the Minimum Supported Rust Version.
I could not get `ruff_macros` to compile on its own so I could not determine its msrv version.

---

_Comment by @messense on 2023-02-19 02:02_

I think we should use the same `rust-version` value, and use [workspace inheritance](https://doc.rust-lang.org/cargo/reference/workspaces.html#the-package-table) to manage it.

---

_Comment by @charliermarsh on 2023-02-19 13:48_

We should use the same value as in `rust-toolchain`. (Is it normal to have both `rust-toolchain` and specify the version in `Cargo.toml`? I assume so since `rust-toolchain` isn't distributed with the crates IIUC.)

---

_Merged by @charliermarsh on 2023-02-19 15:07_

---

_Closed by @charliermarsh on 2023-02-19 15:07_

---

_Branch deleted on 2023-02-23 19:55_

---
