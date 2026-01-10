---
number: 2721
title: "E701 false positive: type annotation"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T17:42:34Z
updated_at: 2023-02-10T20:21:09Z
url: https://github.com/astral-sh/ruff/issues/2721
synced_at: 2026-01-10T01:22:41Z
---

# E701 false positive: type annotation

---

_Issue opened by @spaceone on 2023-02-10 17:42_

`foo: List[str] = []` is detected as E701

---

_Renamed from "E701 false positive" to "E701 false positive: type annotation" by @spaceone on 2023-02-10 17:44_

---

_Comment by @charliermarsh on 2023-02-10 17:52_

This hasn't shipped right? You're running off `main`?

---

_Label `bug` added by @charliermarsh on 2023-02-10 17:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 18:10_

---

_Referenced in [astral-sh/ruff#2737](../../astral-sh/ruff/pulls/2737.md) on 2023-02-10 20:03_

---

_Closed by @charliermarsh on 2023-02-10 20:21_

---
