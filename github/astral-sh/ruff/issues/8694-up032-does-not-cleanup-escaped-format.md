---
number: 8694
title: UP032 does not cleanup escaped format placeholders in string continuations
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2023-11-15T13:30:46Z
updated_at: 2023-11-16T19:59:55Z
url: https://github.com/astral-sh/ruff/issues/8694
synced_at: 2026-01-07T13:12:15-06:00
---

# UP032 does not cleanup escaped format placeholders in string continuations

---

_Issue opened by @ThiefMaster on 2023-11-15 13:30_

Original code:
```python
query = ('type != {} OR '
         "data::text = '{{}}'"
         .format(my_type))
```

```
$ ruff --isolated --select UP032 --preview rufftest/ruff_sample.py
rufftest/ruff_sample.py:1:10: UP032 [*] Use f-string instead of `format` call

$ ruff --isolated --select UP032 --preview rufftest/ruff_sample.py --fix
Found 1 error (1 fixed, 0 remaining).
```

"Fixed" code:
```python
query = (f'type != {my_type} OR '
         "data::text = '{{}}'"
         )
```

Weird whitespace aside, the second string now contains a literal `{{}}` instead of the previous `{}` after formatting.

Any escaped `{{}}` format placeholders in string parts not converted to an f-string should be replaced with their unescaped version `{}`

---

_Referenced in [pytorch/pytorch#113763](../../pytorch/pytorch/pulls/113763.md) on 2023-11-15 14:49_

---

_Comment by @Skylion007 on 2023-11-15 14:51_

We just realized we hit the same bug in PyTorch with ruff a while back when we updated.

---

_Comment by @charliermarsh on 2023-11-15 15:37_

I thought this was filed before and fixed — will look into it!

---

_Label `bug` added by @charliermarsh on 2023-11-15 15:37_

---

_Comment by @zanieb on 2023-11-15 16:01_

Here's a failing test case https://github.com/astral-sh/ruff/pull/8697

---

_Referenced in [astral-sh/ruff#8697](../../astral-sh/ruff/pulls/8697.md) on 2023-11-15 16:17_

---

_Comment by @Skylion007 on 2023-11-15 16:44_

```                        f'"args": {{}}}}, '```
is what broke for me.

---

_Closed by @zanieb on 2023-11-16 19:59_

---
