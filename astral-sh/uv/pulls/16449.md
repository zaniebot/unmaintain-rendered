```yaml
number: 16449
title: Update maturin build backend init template
type: pull_request
state: merged
author: MatthijsKok
labels:
  - enhancement
assignees: []
merged: true
base: main
head: update-maturin-init-template
created_at: 2025-10-25T14:22:47Z
updated_at: 2025-10-27T09:58:17Z
url: https://github.com/astral-sh/uv/pull/16449
synced_at: 2026-01-10T06:36:16Z
```

# Update maturin build backend init template

---

_Pull request opened by @MatthijsKok on 2025-10-25 14:22_

## Summary

Upgrades to the latest Rust edition and pyo3 version. 
Change initialized module to "Inline Declaration" format.
https://pyo3.rs/v0.27.1/module.html#inline-declaration

The output of `maturin new` is also updating to the new declarative format
https://github.com/PyO3/maturin/commit/342239a95a0f3274ad3224e835dddfdb08737e15

## Test Plan

Updated the relevant snapshot tests and to confirm ran
`cargo nextest run --all-features --no-fail-fast maturin`

Also used the updated template to generate a library in a different project with
```
cargo run -- init --lib --build-backend maturin --name try-template ../_OTHER_PROJECT_/backend/try-template
```
and the generated workspace member functioned as expected.

---

_Review requested from @konstin by @zanieb on 2025-10-25 14:24_

---

_Comment by @charliermarsh on 2025-10-27 01:54_

Looks fine to me but defer to @konstin to merge.

---

_Label `enhancement` added by @charliermarsh on 2025-10-27 01:54_

---

_@konstin approved on 2025-10-27 09:57_

Thank you!

---

_Merged by @konstin on 2025-10-27 09:58_

---

_Closed by @konstin on 2025-10-27 09:58_

---
