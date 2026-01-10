```yaml
number: 13645
title: "Include pre-release versions in `uv python install --reinstall`"
type: pull_request
state: merged
author: Michaelgathara
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-reinstall-prerelease
created_at: 2025-05-25T23:10:35Z
updated_at: 2025-05-28T03:00:51Z
url: https://github.com/astral-sh/uv/pull/13645
synced_at: 2026-01-10T11:10:42Z
```

# Include pre-release versions in `uv python install --reinstall`

---

_Pull request opened by @Michaelgathara on 2025-05-25 23:10_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This change allows for `uv python install --reinstall` to include pre-releases when reinstalling. It does this by explicitly allowing pre-releases to be included within `PythonRequest::Any` if the user does not specify a version to reinstall.

Fixes #13582

## Test Plan

<!-- How was it tested? -->
```bash
uv python install 3.14 3.13 3.10
uv python install --no-config --reinstall
```


---

_Review requested from @zanieb by @konstin on 2025-05-26 12:56_

---

_Label `bug` added by @konstin on 2025-05-26 12:56_

---

_@zanieb approved on 2025-05-27 14:35_

Thank you!

---

_Renamed from "Fix: Allow --reinstall to include pre-release versions. Fixes #13582" to "Include pre-release versions in `uv python install --reinstall`" by @zanieb on 2025-05-27 14:35_

---

_Merged by @zanieb on 2025-05-27 14:35_

---

_Closed by @zanieb on 2025-05-27 14:35_

---

_Branch deleted on 2025-05-28 03:00_

---
