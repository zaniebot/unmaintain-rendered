```yaml
number: 3264
title: "Use `macos-12` to build release wheels"
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/macos-12
created_at: 2024-04-25T15:26:34Z
updated_at: 2024-04-25T16:07:31Z
url: https://github.com/astral-sh/uv/pull/3264
synced_at: 2026-01-12T16:05:32Z
```

# Use `macos-12` to build release wheels

---

_@charliermarsh_

## Summary

GitHub has started to change `macos-latest` to `macos-14`. But executables built on `macos-14` don't work on macOS 11 (see: https://github.com/astral-sh/uv/issues/3261). This PR explicitly uses `macos-12` instead (which is what we _intended_ to be using anyway).

Closes https://github.com/astral-sh/uv/issues/3261.


---

_Label `release` added by @charliermarsh on 2024-04-25 15:26_

---

_Marked ready for review by @charliermarsh on 2024-04-25 15:26_

---

_Merged by @charliermarsh on 2024-04-25 16:07_

---

_Closed by @charliermarsh on 2024-04-25 16:07_

---

_Branch deleted on 2024-04-25 16:07_

---
