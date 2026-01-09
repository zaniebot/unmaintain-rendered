---
number: 2582
title: Make TCH005 (empty type checking block) autofixable
type: issue
state: closed
author: LefterisJP
labels:
  - fixes
assignees: []
created_at: 2023-02-05T14:18:09Z
updated_at: 2023-02-05T23:46:09Z
url: https://github.com/astral-sh/ruff/issues/2582
synced_at: 2026-01-07T13:12:14-06:00
---

# Make TCH005 (empty type checking block) autofixable

---

_Issue opened by @LefterisJP on 2023-02-05 14:18_

ruff version: `0.0.237`

After some other auto-fixes I got some empty type checking blocks in a few files. Essentially:

```python
if TYPE_CHECKING:
    pass
```

Running ruff again detected them but it seems it can't autofix them. Only get the error: ` TCH005 Found empty type-checking block`
From a completely naive user perspective it seems easy to implement autofix for this, so just opening an issue for that.

---

_Label `autofix` added by @charliermarsh on 2023-02-05 14:42_

---

_Comment by @charliermarsh on 2023-02-05 14:42_

Ah yeah that's straightforward. Should make it into the next release.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-05 23:44_

---

_Referenced in [astral-sh/ruff#2598](../../astral-sh/ruff/pulls/2598.md) on 2023-02-05 23:46_

---

_Closed by @charliermarsh on 2023-02-05 23:46_

---
