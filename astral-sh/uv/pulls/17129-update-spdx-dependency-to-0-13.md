```yaml
number: 17129
title: Update spdx dependency to 0.13
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: spdx-0.13
created_at: 2025-12-14T11:07:43Z
updated_at: 2025-12-16T11:52:37Z
url: https://github.com/astral-sh/uv/pull/17129
synced_at: 2026-01-12T16:12:37Z
```

# Update spdx dependency to 0.13

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Updates the `spdx` dependency from 0.12.x to the latest release, 0.13.2.

https://github.com/EmbarkStudios/spdx/blob/0.13.2/CHANGELOG.md

Here in uv upstream, this just helps keep dependencies up to date; there isn’t any other particular specific motivation or benefit. Downstream in Fedora, this change allows me to avoid maintaining a `rust-spdx0.12` compat package.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
`cargo nextest run -- --skip python_install::python_install_pyodide`


---

_@charliermarsh approved on 2025-12-14 18:25_

---

_Merged by @charliermarsh on 2025-12-14 18:25_

---

_Closed by @charliermarsh on 2025-12-14 18:25_

---

_Label `internal` added by @konstin on 2025-12-16 11:52_

---
