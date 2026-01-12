```yaml
number: 511
title: C402 false positive with extra kwargs to dict()
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2022-10-29T22:28:29Z
updated_at: 2022-10-29T22:49:53Z
url: https://github.com/astral-sh/ruff/issues/511
synced_at: 2026-01-12T15:54:40Z
```

# C402 false positive with extra kwargs to dict()

---

_@andersk_

`ruff --select=C402` gives “C402 Unnecessary generator - rewrite as a `dict` comprehension” on both of these lines. But the second cannot be rewritten as a `dict` comprehension due to the extra keyword argument `c`, so only the first should be an error.

```python
dict((a, b) for a, b in [("a", "b")])
dict(((a, b) for a, b in [("a", "b")]), c="d")
```

([Examples in real code](https://github.com/zulip/zulip/blob/925fa4ca50215e7d4ba0e7d72b1b77ad3f0b5a38/zerver/lib/exceptions.py#L114-L123). Also reported “upstream” at adamchainz/flake8-comprehensions#457.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-29 22:34_

---

_Label `bug` added by @charliermarsh on 2022-10-29 22:34_

---

_Closed by @charliermarsh on 2022-10-29 22:49_

---

_Comment by @charliermarsh on 2022-10-29 22:49_

Fix is included in https://github.com/charliermarsh/ruff/releases/tag/v0.0.91 (building now).

---
