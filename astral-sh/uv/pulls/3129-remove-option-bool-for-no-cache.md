```yaml
number: 3129
title: "Remove `Option<bool>` for `--no-cache`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/no
created_at: 2024-04-18T22:42:50Z
updated_at: 2024-04-18T22:56:47Z
url: https://github.com/astral-sh/uv/pull/3129
synced_at: 2026-01-12T16:05:26Z
```

# Remove `Option<bool>` for `--no-cache`

---

_@charliermarsh_

## Summary

This was unintended. We ended up reverting `Option<bool>` everywhere, but I missed this once since it's in a separate file.

(If you use `Option<bool>`, Clap requires a value, like `--no-cache true`.)

## Test Plan

`cargo run pip install flask --no-cache`


---

_Renamed from "Remove Option<bool> for --no-cache" to "Remove `Option<bool>` for `--no-cache`" by @charliermarsh on 2024-04-18 22:42_

---

_Label `bug` added by @charliermarsh on 2024-04-18 22:42_

---

_Label `cli` added by @charliermarsh on 2024-04-18 22:42_

---

_Marked ready for review by @charliermarsh on 2024-04-18 22:43_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-18 22:43_

---

_@zanieb approved on 2024-04-18 22:44_

---

_Merged by @charliermarsh on 2024-04-18 22:56_

---

_Closed by @charliermarsh on 2024-04-18 22:56_

---

_Branch deleted on 2024-04-18 22:56_

---
