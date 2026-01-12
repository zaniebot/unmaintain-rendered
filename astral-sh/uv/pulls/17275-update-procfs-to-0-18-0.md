```yaml
number: 17275
title: Update procfs to 0.18.0
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
  - dependencies
assignees: []
merged: true
base: main
head: procfs-0.18
created_at: 2025-12-30T23:31:29Z
updated_at: 2026-01-06T09:51:30Z
url: https://github.com/astral-sh/uv/pull/17275
synced_at: 2026-01-12T16:12:41Z
```

# Update procfs to 0.18.0

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This brings various minor improvements described in https://github.com/eminence/procfs/releases/tag/v0.18.0, avoids duplicate rustix versions in Cargo.lock, and offers upstream the change that we’re making in Fedora’s `uv` package to avoid shipping a `rust-procfs0.17` compat package.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
`cargo nextest run -- --skip python_install_pyodide`

---

_@charliermarsh approved on 2026-01-01 13:25_

Great, thanks!

---

_Label `dependencies` added by @charliermarsh on 2026-01-01 13:26_

---

_Merged by @charliermarsh on 2026-01-01 13:36_

---

_Closed by @charliermarsh on 2026-01-01 13:36_

---

_Label `internal` added by @konstin on 2026-01-06 09:51_

---
