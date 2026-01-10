```yaml
number: 4247
title: "Make missing `METADATA` file a recoverable error"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/e
created_at: 2024-06-11T19:21:06Z
updated_at: 2024-06-12T07:36:56Z
url: https://github.com/astral-sh/uv/pull/4247
synced_at: 2026-01-10T13:54:02Z
```

# Make missing `METADATA` file a recoverable error

---

_Pull request opened by @charliermarsh on 2024-06-11 19:21_

## Summary

I don't have a great way to test it, but this makes the error described in https://github.com/astral-sh/uv/issues/4246 an incompatibility rather than a fatal error.

Closes https://github.com/astral-sh/uv/issues/4246.


---

_Label `bug` added by @charliermarsh on 2024-06-11 19:21_

---

_Marked ready for review by @charliermarsh on 2024-06-11 19:21_

---

_@zanieb approved on 2024-06-11 19:34_

Cool

---

_Merged by @charliermarsh on 2024-06-11 19:49_

---

_Closed by @charliermarsh on 2024-06-11 19:49_

---

_Branch deleted on 2024-06-11 19:49_

---

_Comment by @jcugat on 2024-06-12 07:36_

Can confirm that uv `0.2.11` solves the issue I raised in https://github.com/astral-sh/uv/issues/4246 with this fix, thanks!

---
