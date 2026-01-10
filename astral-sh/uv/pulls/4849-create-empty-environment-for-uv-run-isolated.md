```yaml
number: 4849
title: "Create empty environment for `uv run --isolated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/iso
created_at: 2024-07-06T19:13:02Z
updated_at: 2024-07-06T19:24:02Z
url: https://github.com/astral-sh/uv/pull/4849
synced_at: 2026-01-10T13:48:28Z
```

# Create empty environment for `uv run --isolated`

---

_Pull request opened by @charliermarsh on 2024-07-06 19:13_

## Summary

If you pass `--isolated` but no `--with`, at present, we don't create any environment (so `--python` isn't respected and `python` will fail entirely if it wasn't already in your path). Now, we create a base environment in `--isolated` even if `with` wasn't provided.

Closes https://github.com/astral-sh/uv/issues/4846.
Closes https://github.com/astral-sh/uv/issues/4776.


---

_Label `bug` added by @charliermarsh on 2024-07-06 19:13_

---

_Label `preview` added by @charliermarsh on 2024-07-06 19:13_

---

_Merged by @charliermarsh on 2024-07-06 19:22_

---

_Closed by @charliermarsh on 2024-07-06 19:22_

---

_Branch deleted on 2024-07-06 19:22_

---

_Comment by @zanieb on 2024-07-06 19:24_

Nice thanks!

---
