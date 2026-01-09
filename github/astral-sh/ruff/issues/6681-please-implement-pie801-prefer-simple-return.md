---
number: 6681
title: Please implement PIE801 (prefer-simple-return)
type: issue
state: closed
author: nth10sd
labels:
  - rule
assignees: []
created_at: 2023-08-18T19:54:50Z
updated_at: 2024-04-12T02:09:06Z
url: https://github.com/astral-sh/ruff/issues/6681
synced_at: 2026-01-07T13:12:15-06:00
---

# Please implement PIE801 (prefer-simple-return)

---

_Issue opened by @nth10sd on 2023-08-18 19:54_

From [flake8-pie](https://github.com/sbdchd/flake8-pie):

```
Return boolean expressions directly instead of returning True and False.

# error
def main():
    if foo > 5:
        return True
    return False

# error
def main():
    if foo > 5:
        return True
    else:
        return False

# ok
def main():
    return foo > 5
```

```
$ ruff --version
ruff 0.0.285
```

---

_Comment by @charliermarsh on 2023-08-18 20:03_

Is this the same as https://beta.ruff.rs/docs/rules/needless-bool/?

---

_Comment by @nth10sd on 2023-08-18 20:15_

It's similar.

However, the following testcase is detected as `PIE801` via `flake8-pie` but not by `ruff`:

```
def f():
    x = "x"
    y = "y"
    if x == "x" and y == "z":
        return True
    return False
```

---

_Comment by @dhruvmanila on 2023-08-20 03:21_

Currently, the [`needless-bool`](https://beta.ruff.rs/docs/rules/needless-bool/) works for an `if` statement _only_. Here, `return False` is _outside_ the `if` statement and so it isn't detected here. If it would be inside an `else` body, then we would detect it (https://play.ruff.rs/43e800ce-14d3-41f5-9e9e-1d86b4ae5d19). I'm not sure if there's a way to get the next statement without using the entire function body.

---

_Comment by @charliermarsh on 2023-08-20 13:58_

We do something similar in `convert_for_loop_to_any_all`, to get the next sibling.

---

_Label `rule` added by @charliermarsh on 2023-09-21 00:50_

---

_Comment by @charliermarsh on 2024-04-12 02:09_

These are now picked up by [`needless-bool`](https://docs.astral.sh/ruff/rules/needless-bool/) in preview.

---

_Closed by @charliermarsh on 2024-04-12 02:09_

---
