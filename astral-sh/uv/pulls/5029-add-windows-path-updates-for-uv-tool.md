```yaml
number: 5029
title: "Add Windows path updates for `uv tool`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - windows
  - preview
assignees: []
merged: true
base: main
head: charlie/winreg
created_at: 2024-07-13T01:29:12Z
updated_at: 2024-07-13T01:55:06Z
url: https://github.com/astral-sh/uv/pull/5029
synced_at: 2026-01-10T13:42:52Z
```

# Add Windows path updates for `uv tool`

---

_Pull request opened by @charliermarsh on 2024-07-13 01:29_

## Summary

Largely based on rustup's implementation (linked in the source).

Closes #5027.

## Test Plan

- Changed the executable directory to `uv/foo`.
- Ran script; verified that I could access executables in `foo`.


---

_Label `enhancement` added by @charliermarsh on 2024-07-13 01:29_

---

_Label `windows` added by @charliermarsh on 2024-07-13 01:29_

---

_Label `preview` added by @charliermarsh on 2024-07-13 01:29_

---

_@charliermarsh reviewed on 2024-07-13 01:54_

---

_Review comment by @charliermarsh on `Cargo.toml`:154 on 2024-07-13 01:54_

Already in our dependency tree. I'd slightly rather use `winapi` if possible but `rustup` uses `winreg` and it's just a pain to translate it.

---

_Merged by @charliermarsh on 2024-07-13 01:55_

---

_Closed by @charliermarsh on 2024-07-13 01:55_

---

_Branch deleted on 2024-07-13 01:55_

---
