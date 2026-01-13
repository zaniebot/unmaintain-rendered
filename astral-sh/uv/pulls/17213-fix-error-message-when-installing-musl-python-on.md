```yaml
number: 17213
title: Fix error message when installing musl python on armv7
type: pull_request
state: merged
author: dijor0310
labels: []
assignees: []
merged: true
base: main
head: diyor/fix-python-install-error
created_at: 2025-12-22T00:42:11Z
updated_at: 2026-01-13T02:45:05Z
url: https://github.com/astral-sh/uv/pull/17213
synced_at: 2026-01-13T03:20:00Z
```

# Fix error message when installing musl python on armv7

---

_@dijor0310_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #17210.

## Test Plan

Added a test function.

---

_Comment by @zanieb on 2025-12-22 15:59_

I think we actually need to retain the hint for armv7

---

_Comment by @zanieb on 2025-12-22 16:00_

(We don't run tests on armv7 so we can't actually validate the change that way)

---

_Renamed from "Fix obsolete error message when installing python on arm" to "Fix obsolete error message when installing python on armv7" by @dijor0310 on 2025-12-22 23:48_

---

_Comment by @dijor0310 on 2025-12-22 23:52_

@zanieb refactored the original message to say that `uv does not yet provide musl Python distributions on armv7.` and added a test, please take another look.

---

_Renamed from "Fix obsolete error message when installing python on armv7" to "Fix error message when installing musl python on armv7" by @dijor0310 on 2025-12-22 23:57_

---

_Comment by @dijor0310 on 2026-01-08 16:45_

Hi @zanieb, just bumping this thread. No rush at all, but let me know if I can further improve the PR or whether you need it at all.

---

_@zanieb approved on 2026-01-13 02:15_

---

_Merged by @zanieb on 2026-01-13 02:45_

---

_Closed by @zanieb on 2026-01-13 02:45_

---
