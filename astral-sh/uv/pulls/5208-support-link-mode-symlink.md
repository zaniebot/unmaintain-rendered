```yaml
number: 5208
title: "Support  `--link-mode=symlink`"
type: pull_request
state: merged
author: danielenricocahall
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: feature/add-symlink-capability
created_at: 2024-07-19T01:41:08Z
updated_at: 2024-07-19T12:41:18Z
url: https://github.com/astral-sh/uv/pull/5208
synced_at: 2026-01-12T16:06:42Z
```

# Support  `--link-mode=symlink`

---

_@danielenricocahall_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Addressing this [issue](https://github.com/astral-sh/uv/issues/5147)  by adding the capability for Symbolic linking as a link mode when installing or syncing dependencies.

## Test Plan

<!-- How was it tested? -->


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-19 01:53_

---

_@charliermarsh reviewed on 2024-07-19 01:56_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:583 on 2024-07-19 01:56_

Do you mind adding the whole `Attempt::UseCopyFallback` thing from `hardlink_wheel_files`? It's helpful because if we detect that symlinks aren't supported, we basically stop trying.

---

_@charliermarsh reviewed on 2024-07-19 01:56_

Thank you! This looks great. I'll merge tomorrow when GitHub Actions is back online.

---

_Label `enhancement` added by @charliermarsh on 2024-07-19 01:56_

---

_Label `cli` added by @charliermarsh on 2024-07-19 01:56_

---

_@danielenricocahall reviewed on 2024-07-19 02:11_

---

_Review comment by @danielenricocahall on `crates/install-wheel-rs/src/linker.rs`:583 on 2024-07-19 02:11_

Done! Relegated this function to just being a copy of the hardlink function

---

_@charliermarsh approved on 2024-07-19 12:27_

Thanks!

---

_Merged by @charliermarsh on 2024-07-19 12:41_

---

_Closed by @charliermarsh on 2024-07-19 12:41_

---
