```yaml
number: 14488
title: "trim content of `INSTALLER` file"
type: pull_request
state: merged
author: nilskch
labels:
  - internal
assignees: []
merged: true
base: main
head: nk/trim-installer
created_at: 2025-07-07T16:00:32Z
updated_at: 2025-07-07T16:19:31Z
url: https://github.com/astral-sh/uv/pull/14488
synced_at: 2026-01-10T06:53:02Z
```

# trim content of `INSTALLER` file

---

_Pull request opened by @nilskch on 2025-07-07 16:00_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We are using UV as a library and `installer()` returned `"pip\n"`. The packages got installed by the pip package manager and not by UV. pip seems to add a new line to the `INSTALLER` file and UV does not.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @nilskch on 2025-07-07 16:09_

---

_Comment by @konstin on 2025-07-07 16:15_

We could probably write a newline for pip too, adding a newline to the last line of a file is a good practice, but I disgress.

---

_@konstin approved on 2025-07-07 16:16_

---

_Merged by @konstin on 2025-07-07 16:16_

---

_Closed by @konstin on 2025-07-07 16:16_

---

_Label `internal` added by @konstin on 2025-07-07 16:16_

---

_Branch deleted on 2025-07-07 16:19_

---
