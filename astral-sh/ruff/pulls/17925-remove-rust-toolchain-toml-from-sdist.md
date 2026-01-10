```yaml
number: 17925
title: Remove rust-toolchain.toml from sdist
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - release
assignees: []
merged: true
base: brent/release-0.12.0
head: micha/rust-toolchain-sdist
created_at: 2025-05-07T16:56:25Z
updated_at: 2025-06-12T07:20:05Z
url: https://github.com/astral-sh/ruff/pull/17925
synced_at: 2026-01-10T18:45:04Z
```

# Remove rust-toolchain.toml from sdist

---

_Pull request opened by @MichaReiser on 2025-05-07 16:56_

## Summary
The `rust-toolchain.toml` specifies the rust toolchain version that we use for development. 
Consumers of the `ruff` package can use any Rust version (that meets our MSRV) to build ruff from an sdist. 

Closes https://github.com/astral-sh/ruff/issues/17909

## Test Plan

Ran `uv build` and verified that the `rust-toolchain.toml` is no longer present in the `sdist` folder. 

I uninstalled all Rust toolchains and verified that `cargo build` re-installs the latest stable version. 

I verified that `cargo build` automatically installs the latest stable if the default Rust toolchain doesn't meet the MSRV.


---

_Label `release` added by @MichaReiser on 2025-05-07 16:58_

---

_Comment by @MichaReiser on 2025-05-07 17:06_

@ntBre I don't think this should require any changes on the conda forge side but what changes were needed last time we bumped the MSRV/rust-toolchain?

---

_Marked ready for review by @MichaReiser on 2025-05-07 17:06_

---

_Comment by @ntBre on 2025-05-07 17:10_

I just had to bump the `rust_compiler_version` in the main recipe and also in one of the Windows recipes. And I guess bump the build number, but I think that's for any change.

My last PR: https://github.com/conda-forge/ruff-feedstock/pull/266
Zanie's PR I based it on: https://github.com/conda-forge/ruff-feedstock/pull/252

---

_Comment by @MichaReiser on 2025-05-07 17:11_

Thanks. 

---

_Added to milestone `v0.12` by @MichaReiser on 2025-05-17 17:04_

---

_Comment by @ntBre on 2025-06-10 20:14_

Do we want to try to merge this into the 0.12 branch?

---

_Merged by @MichaReiser on 2025-06-11 06:50_

---

_Closed by @MichaReiser on 2025-06-11 06:50_

---

_Branch deleted on 2025-06-11 06:50_

---

_Label `breaking` added by @MichaReiser on 2025-06-11 06:51_

---
