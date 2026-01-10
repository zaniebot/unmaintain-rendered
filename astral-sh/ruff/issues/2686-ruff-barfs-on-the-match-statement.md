```yaml
number: 2686
title: "Ruff barfs on the `match` statement"
type: issue
state: closed
author: sfermigier
labels: []
assignees: []
created_at: 2023-02-09T13:58:50Z
updated_at: 2023-02-22T00:04:47Z
url: https://github.com/astral-sh/ruff/issues/2686
synced_at: 2026-01-10T11:09:45Z
```

# Ruff barfs on the `match` statement

---

_Issue opened by @sfermigier on 2023-02-09 13:58_

Starting with version 0.0.241, ruff rejects code containing `match` statements like:

```python
x = 0

match x:
  case 1: print("hello")
```

With error like:

```
error: Failed to parse toto.py: invalid syntax. Got unexpected token 'x' at line 3 column 7
```

(this passes with ruff 0.0.240).


---

_Comment by @g-as on 2023-02-09 14:16_

It wasn't working before, it's just that since 0.0.241, you know about it ;)

#2505 was shipped in 0.0.241.

See the discussion in the PR.

---

_Comment by @g-as on 2023-02-09 14:18_

Note that `ruff` exits 0 in both versions.

---

_Comment by @g-as on 2023-02-09 14:19_

See #282.

---

_Comment by @charliermarsh on 2023-02-09 14:25_

Yeah this is intended behavior. It was failing silently for you before.

---

_Closed by @charliermarsh on 2023-02-09 14:25_

---

_Comment by @charliermarsh on 2023-02-22 00:04_

Fixed as of v0.0.250.

---
