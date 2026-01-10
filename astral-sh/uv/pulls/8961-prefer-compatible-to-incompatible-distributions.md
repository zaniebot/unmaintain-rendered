```yaml
number: 8961
title: Prefer compatible to incompatible distributions when packages exist on multiple indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: charlie/incompat
created_at: 2024-11-09T00:27:33Z
updated_at: 2024-11-09T00:48:50Z
url: https://github.com/astral-sh/uv/pull/8961
synced_at: 2026-01-10T12:00:00Z
```

# Prefer compatible to incompatible distributions when packages exist on multiple indexes

---

_Pull request opened by @charliermarsh on 2024-11-09 00:27_

## Summary

At time of writing, `markupsafe==3.0.2` exists on the PyTorch index, but there's
only a single wheel:

`MarkupSafe-3.0.2-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl`

Meanwhile, there are a large number of wheels on PyPI for the same version. If the
user is on Python 3.12, and we return the incompatible PyTorch wheel without
considering the PyPI wheels, PubGrub will mark 3.0.2 as an incompatible version,
even though there are compatible wheels on PyPI.

Closes https://github.com/astral-sh/uv/issues/8922.


---

_Label `bug` added by @charliermarsh on 2024-11-09 00:27_

---

_Label `resolver` added by @charliermarsh on 2024-11-09 00:27_

---

_Comment by @charliermarsh on 2024-11-09 00:33_

(Figuring out how to test this.)

---

_Merged by @charliermarsh on 2024-11-09 00:48_

---

_Closed by @charliermarsh on 2024-11-09 00:48_

---

_Branch deleted on 2024-11-09 00:48_

---
