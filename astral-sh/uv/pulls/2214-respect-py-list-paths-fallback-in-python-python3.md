```yaml
number: 2214
title: "Respect `py --list-paths` fallback in `--python python3` invocations"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-03-05T19:06:29Z
updated_at: 2024-03-05T19:28:25Z
url: https://github.com/astral-sh/uv/pull/2214
synced_at: 2026-01-10T14:54:43Z
```

# Respect `py --list-paths` fallback in `--python python3` invocations

---

_Pull request opened by @charliermarsh on 2024-03-05 19:06_

## Summary

This makes `--python python3` and `--python 3.10` more consistent on Windows.

Closes https://github.com/astral-sh/uv/issues/2213.

## Test Plan

Ran `cargo run venv --python python3.12` with the Windows Store Python.


---

_Marked ready for review by @charliermarsh on 2024-03-05 19:06_

---

_Label `bug` added by @charliermarsh on 2024-03-05 19:06_

---

_Label `windows` added by @charliermarsh on 2024-03-05 19:06_

---

_Converted to draft by @charliermarsh on 2024-03-05 19:08_

---

_Marked ready for review by @charliermarsh on 2024-03-05 19:11_

---

_Merged by @charliermarsh on 2024-03-05 19:28_

---

_Closed by @charliermarsh on 2024-03-05 19:28_

---

_Branch deleted on 2024-03-05 19:28_

---
