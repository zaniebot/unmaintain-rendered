---
number: 10029
title: "`UP018` doesn't detect integers literals used with unary operators"
type: issue
state: closed
author: dosisod
labels:
  - bug
assignees: []
created_at: 2024-02-18T19:59:13Z
updated_at: 2024-02-20T18:21:08Z
url: https://github.com/astral-sh/ruff/issues/10029
synced_at: 2026-01-07T13:12:15-06:00
---

# `UP018` doesn't detect integers literals used with unary operators

---

_Issue opened by @dosisod on 2024-02-18 19:59_

UP018 doesn't warn about integer literals when prefixed with unary operators:

```python
_ = int(1)
_ = int(-1)
_ = int(+1)
_ = int(~1)
```
Running:
```
$ ruff --version
ruff 0.2.2

$ ruff file.py --output-format=concise
file.py:1:5: UP018 [*] Unnecessary `int` call (rewrite as a literal)
```

Only the first line is detected, but all 4 lines should emit errors.

---

_Label `bug` added by @charliermarsh on 2024-02-20 05:49_

---

_Comment by @charliermarsh on 2024-02-20 05:49_

Agreed.

---

_Comment by @sanxiyn on 2024-02-20 12:00_

I am not sure about `~1`. It is bad, but `~1` is not a literal, so I think it is out of scope for UP018.

---

_Referenced in [astral-sh/ruff#10060](../../astral-sh/ruff/pulls/10060.md) on 2024-02-20 12:01_

---

_Comment by @trag1c on 2024-02-20 12:22_

> I am not sure about `~1`. It is bad, but `~1` is not a literal, so I think it is out of scope for UP018.

Why is `-1` a literal but `~1` isn't? Both have the same AST structure

---

_Comment by @sanxiyn on 2024-02-20 12:51_

I mean, `-1` is not a literal either in terms of AST, but you could say it is a literal in spirit, since it evaluates to itself. That doesn't apply to `~1`, which evaluates to `-2`. If `~1` is a literal, `60 * 60` is a literal too.

---

_Comment by @trag1c on 2024-02-20 12:53_

> I mean, `-1` is not a literal either in terms of AST, but you could say it is a literal in spirit, since it evaluates to itself. That doesn't apply to `~1`, which evaluates to `-2`. If `~1` is a literal, `60 * 60` is a literal too.

That's fair 👍

---

_Closed by @charliermarsh on 2024-02-20 18:21_

---
