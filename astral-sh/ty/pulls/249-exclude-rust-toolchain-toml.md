```yaml
number: 249
title: Exclude rust-toolchain.toml
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: micha/exclude-rust-toolchain
created_at: 2025-05-07T16:51:58Z
updated_at: 2025-05-07T17:49:05Z
url: https://github.com/astral-sh/ty/pull/249
synced_at: 2026-01-12T15:54:27Z
```

# Exclude rust-toolchain.toml

---

_@MichaReiser_

## Summary
The `rust-toolchain.toml` specifies the rust toolchain version that we use for development. 
Consumers of the `ty` package can use any Rust versiono (that meets our MSRV) to build ty from an sdist. 

## Test Plan

Ran `uv build` and verified that the `rust-toolchain.toml` is no longer present in the `sdist` folder. 

I uninstalled all Rust toolchains and verified that `cargo build` re-installs the latest stable version. 

I verified that `cargo build` automatically installs the latest stable if the default Rust toolchain doesn't meet the MSRV.


---

_Comment by @zanieb on 2025-05-07 17:01_

Did you mean to update the submodule?

---

_Comment by @MichaReiser on 2025-05-07 17:03_

No. I hate sub modules 😆 

---

_Merged by @zanieb on 2025-05-07 17:49_

---

_Closed by @zanieb on 2025-05-07 17:49_

---

_Branch deleted on 2025-05-07 17:49_

---
