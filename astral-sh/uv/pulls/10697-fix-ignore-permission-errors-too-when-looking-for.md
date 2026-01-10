```yaml
number: 10697
title: "fix: ignore permission errors too when looking for user file"
type: pull_request
state: merged
author: henryiii
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-01-16T22:28:24Z
updated_at: 2025-01-17T20:50:30Z
url: https://github.com/astral-sh/uv/pull/10697
synced_at: 2026-01-10T11:45:04Z
```

# fix: ignore permission errors too when looking for user file

---

_Pull request opened by @henryiii on 2025-01-16 22:28_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The new ARM runners report a permission error:

```
Run uvx twine check wheelhouse/*
error: failed to open file `/home/runneradmin/.config/uv/uv.toml`: Permission denied (os error 13)
```

In this PR, a PermissionsError is treated like not finding the file.

I reworked the structure just a bit to avoid calling `err.kind()` multiple times.

## Test Plan

<!-- How was it tested? -->

Added a UNIX only test where I set the permissions of the folder containing the file and try to find it.

---

_Marked ready for review by @henryiii on 2025-01-17 15:28_

---

_@charliermarsh approved on 2025-01-17 17:27_

---

_Label `bug` added by @charliermarsh on 2025-01-17 17:27_

---

_Merged by @charliermarsh on 2025-01-17 17:28_

---

_Closed by @charliermarsh on 2025-01-17 17:28_

---

_Comment by @charliermarsh on 2025-01-17 17:28_

Thanks @henryiii!

---

_Comment by @henryiii on 2025-01-17 20:50_

FYI, I've heard bad permissions is a known bug of the new runners and they'll be fixing it in the future. But still probably a good improvement.

---
