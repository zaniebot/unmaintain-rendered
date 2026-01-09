---
number: 6684
title: Question about SIM201
type: issue
state: closed
author: nth10sd
labels: []
assignees: []
created_at: 2023-08-18T20:07:23Z
updated_at: 2023-08-18T20:26:45Z
url: https://github.com/astral-sh/ruff/issues/6684
synced_at: 2026-01-07T13:12:15-06:00
---

# Question about SIM201

---

_Issue opened by @nth10sd on 2023-08-18 20:07_

This testcase does not trigger SIM201:

```
x = "x"
y = "y"

if not x == "x" or not y == "y":
    raise RuntimeError
```

However, changing `raise` to `pass` causes `ruff` to detect SIM201 as expected:

```
x = "x"
y = "y"

if not x == "x" or not y == "y":
    pass
```

```
test.py:4:4: SIM201 [*] Use `x != "x"` instead of `not x == "x"`
test.py:4:20: SIM201 [*] Use `y != "y"` instead of `not y == "y"`
```

May I know if this is expected?

```
$ ruff --version
ruff 0.0.285
```

---

_Comment by @charliermarsh on 2023-08-18 20:16_

It is expected and flake8-simplify has a similar exemption (though it doesn't apply to boolean ops in flake8-simplify). I believe it's documented here in flake8-simplify: https://github.com/MartinThoma/flake8-simplify/issues/13

---

_Comment by @nth10sd on 2023-08-18 20:26_

Got it, thanks!

---

_Closed by @nth10sd on 2023-08-18 20:26_

---

_Referenced in [astral-sh/ruff#9106](../../astral-sh/ruff/issues/9106.md) on 2023-12-12 11:12_

---
