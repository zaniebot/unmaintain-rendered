```yaml
number: 1406
title: "Avoid import contextlib in `_virtualenv`"
type: pull_request
state: merged
author: hauntsaninja
labels:
  - performance
assignees: []
merged: true
base: main
head: avoid-contextlib
created_at: 2024-02-16T01:14:02Z
updated_at: 2024-02-16T01:43:42Z
url: https://github.com/astral-sh/uv/pull/1406
synced_at: 2026-01-10T15:33:24Z
```

# Avoid import contextlib in `_virtualenv`

---

_Pull request opened by @hauntsaninja on 2024-02-16 01:14_

No need to pay 3ms on basically every Python invocation. I opened a PR upstream last week: https://github.com/pypa/virtualenv/pull/2688

---

_@charliermarsh approved on 2024-02-16 01:14_

---

_Comment by @charliermarsh on 2024-02-16 01:14_

Nice, thank you.

---

_@zanieb approved on 2024-02-16 01:15_

Thanks!

---

_Label `performance` added by @zanieb on 2024-02-16 01:15_

---

_Merged by @charliermarsh on 2024-02-16 01:23_

---

_Closed by @charliermarsh on 2024-02-16 01:23_

---

_Branch deleted on 2024-02-16 01:43_

---
