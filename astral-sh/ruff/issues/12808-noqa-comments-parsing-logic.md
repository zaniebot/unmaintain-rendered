```yaml
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
synced_at: 2026-01-12T15:54:52Z
```

# `noqa` comments parsing logic

---

_@InSyncWithFoo_

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

_Closed by @charliermarsh on 2024-11-02 20:25_

---
