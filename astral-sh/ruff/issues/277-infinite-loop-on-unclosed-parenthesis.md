---
number: 277
title: Infinite loop on unclosed parenthesis
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2022-09-28T22:32:47Z
updated_at: 2022-09-29T02:06:37Z
url: https://github.com/astral-sh/ruff/issues/277
synced_at: 2026-01-10T01:22:37Z
---

# Infinite loop on unclosed parenthesis

---

_Issue opened by @andersk on 2022-09-28 22:32_

If you run ruff on this file with an unclosed parenthesis:

```python
print(
```

it loops forever allocating several GB of memory per second. This regression was introduced by 38b19b78b737ee3f2c0f781fb14950f4d4a97326. The problem is that

https://github.com/charliermarsh/ruff/blob/886def13bd2cfc2bfb20ad3c0e42991f5ac1e2cb/src/linter.rs#L87

produces an infinite stream of `Err(LexicalError { error: Eof, location: Location { row: 2, column: 1 } })` repeating forever, which doesn’t fit easily into a `Vec`.

---

_Label `bug` added by @charliermarsh on 2022-09-28 23:00_

---

_Comment by @charliermarsh on 2022-09-28 23:15_

Oh good catch, thanks for filing. I wonder why this didn’t error before. Will take a look.

---

_Referenced in [astral-sh/ruff#279](../../astral-sh/ruff/pulls/279.md) on 2022-09-29 01:15_

---

_Closed by @charliermarsh on 2022-09-29 02:06_

---
