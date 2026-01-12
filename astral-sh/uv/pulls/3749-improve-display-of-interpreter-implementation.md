```yaml
number: 3749
title: Improve display of interpreter implementation names
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - tracing
assignees: []
merged: true
base: main
head: zb/implementation-display
created_at: 2024-05-22T18:07:30Z
updated_at: 2024-05-22T18:22:10Z
url: https://github.com/astral-sh/uv/pull/3749
synced_at: 2026-01-12T16:05:50Z
```

# Improve display of interpreter implementation names

---

_@zanieb_

e.g. instead of

```
❯ uv venv --python pypy@3.10
  × No interpreter found for pypy 3.10 in search path
```

we say

```
❯ uv venv --python pypy@3.10
  × No interpreter found for PyPy 3.10 in search path
```


---

_Label `error messages` added by @zanieb on 2024-05-22 18:07_

---

_Label `tracing` added by @zanieb on 2024-05-22 18:07_

---

_Merged by @zanieb on 2024-05-22 18:22_

---

_Closed by @zanieb on 2024-05-22 18:22_

---

_Branch deleted on 2024-05-22 18:22_

---
