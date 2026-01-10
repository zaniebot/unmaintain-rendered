```yaml
number: 12180
title: "Ensure `python pin --global` creates parent directories if missing"
type: pull_request
state: merged
author: samypr100
labels:
  - bug
assignees: []
merged: true
base: main
head: python-pin-dir-fix
created_at: 2025-03-15T01:42:43Z
updated_at: 2025-03-15T03:14:40Z
url: https://github.com/astral-sh/uv/pull/12180
synced_at: 2026-01-10T11:10:39Z
```

# Ensure `python pin --global` creates parent directories if missing

---

_Pull request opened by @samypr100 on 2025-03-15 01:42_

## Summary

Closes https://github.com/astral-sh/uv/issues/12178

## Test Plan

Added new test. Manually tested on Windows and Linux.

---

_Marked ready for review by @samypr100 on 2025-03-15 01:52_

---

_@charliermarsh approved on 2025-03-15 02:42_

---

_Renamed from "Ensure python pin --global creates parent directories if missing" to "Ensure `python pin --global` creates parent directories if missing" by @charliermarsh on 2025-03-15 02:42_

---

_Label `bug` added by @charliermarsh on 2025-03-15 02:42_

---

_Comment by @charliermarsh on 2025-03-15 02:42_

Thanks!

---

_Merged by @charliermarsh on 2025-03-15 02:50_

---

_Closed by @charliermarsh on 2025-03-15 02:50_

---

_Branch deleted on 2025-03-15 03:06_

---

_@samypr100 reviewed on 2025-03-15 03:14_

---

_Review comment by @samypr100 on `crates/uv/src/commands/python/pin.rs`:148 on 2025-03-15 03:14_

Thanks. Weird, I had this originally but clippy was complaining about it which was odd. Maybe there was something else going on that I didn't notice.

---
