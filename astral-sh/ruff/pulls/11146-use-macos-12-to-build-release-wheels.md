```yaml
number: 11146
title: "Use `macos-12` to build release wheels"
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/macos
created_at: 2024-04-25T15:25:39Z
updated_at: 2024-04-25T16:07:26Z
url: https://github.com/astral-sh/ruff/pull/11146
synced_at: 2026-01-10T22:37:01Z
```

# Use `macos-12` to build release wheels

---

_Pull request opened by @charliermarsh on 2024-04-25 15:25_

## Summary

GitHub has started to change `macos-latest` to `macos-14`. But executables built on `macos-14` don't work on macOS 11 (see: https://github.com/astral-sh/uv/issues/3261). This PR explicitly uses `macos-12` instead (which is what we _intended_ to be using anyway).


---

_Label `release` added by @charliermarsh on 2024-04-25 15:25_

---

_Merged by @charliermarsh on 2024-04-25 16:07_

---

_Closed by @charliermarsh on 2024-04-25 16:07_

---

_Branch deleted on 2024-04-25 16:07_

---
