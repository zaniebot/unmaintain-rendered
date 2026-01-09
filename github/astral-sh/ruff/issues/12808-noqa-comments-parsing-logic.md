---
number: 12808
title: "`noqa` comments parsing logic"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - bug
  - suppression
assignees: []
created_at: 2024-08-11T22:57:41Z
updated_at: 2024-11-02T20:25:01Z
url: https://github.com/astral-sh/ruff/issues/12808
synced_at: 2026-01-07T13:12:15-06:00
---

# `noqa` comments parsing logic

---

_Issue opened by @InSyncWithFoo on 2024-08-11 22:57_

Currently these are all valid `noqa` comments:

```py
# noqa: A100,B101
# noqa: A100 B101
# noqa: A100, B101
# noqa: A100 , B101
```

```py
# noqa: A100B101
# noqa: A100,,B101
```

The formats in the second group should be invalid.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-11 22:58_

---

_Label `bug` added by @charliermarsh on 2024-08-11 22:58_

---

_Label `suppression` added by @charliermarsh on 2024-08-11 22:58_

---

_Comment by @charliermarsh on 2024-08-11 22:59_

I think `noqa: A100,,B101` is maybe ok since it's at least unambiguous (but not currently formatted).

---

_Comment by @charliermarsh on 2024-08-11 22:59_

`# noqa: A100B101` is definitely wrong.

---

_Referenced in [astral-sh/ruff#12809](../../astral-sh/ruff/pulls/12809.md) on 2024-08-11 23:07_

---

_Referenced in [astral-sh/ruff#12810](../../astral-sh/ruff/issues/12810.md) on 2024-08-11 23:17_

---

_Closed by @charliermarsh on 2024-11-02 20:25_

---
