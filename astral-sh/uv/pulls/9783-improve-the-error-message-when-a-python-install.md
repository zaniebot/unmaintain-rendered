```yaml
number: 9783
title: Improve the error message when a Python install request is not valid
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/err
created_at: 2024-12-10T19:14:27Z
updated_at: 2024-12-10T20:20:48Z
url: https://github.com/astral-sh/uv/pull/9783
synced_at: 2026-01-12T16:08:58Z
```

# Improve the error message when a Python install request is not valid

---

_@zanieb_

```
❯ uv python install foo
error: Cannot download managed Python for request: directory `foo`
❯ cargo run -q -- python install foo
error: `foo` is not a valid Python download request; see `uv python help` for supported formats and `uv python list --only-downloads` for available versions
```

---

_Label `enhancement` added by @zanieb on 2024-12-10 19:14_

---

_Renamed from "Improve the error message when an install request is not valid" to "Improve the error message when a Python install request is not valid" by @zanieb on 2024-12-10 19:17_

---

_@charliermarsh approved on 2024-12-10 19:58_

---

_Merged by @zanieb on 2024-12-10 20:20_

---

_Closed by @zanieb on 2024-12-10 20:20_

---

_Branch deleted on 2024-12-10 20:20_

---
