---
number: 2229
title: PD011 in random code
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-26T21:47:33Z
updated_at: 2023-09-18T09:09:57Z
url: https://github.com/astral-sh/ruff/issues/2229
synced_at: 2026-01-07T13:12:14-06:00
---

# PD011 in random code

---

_Issue opened by @spaceone on 2023-01-26 21:47_

I had a function such as:
```
$ cat foo.py
def foo(parser):
        parser.values.foo

$ ruff --select PD011 --fix foo.py
foo.py:2:2: PD011 Use `.to_numpy()` instead of `.values`
Found 1 error.
```

I don't use pandas.
I don't know the details of this check. Maybe further access can be checked? Or if `pandas` is imported in the code?
Otherwise just close the issue and I add this to the ignore list.

---

_Comment by @charliermarsh on 2023-01-26 21:48_

I thought we did do something like this (check if Pandas is imported). Let me check...

---

_Label `bug` added by @charliermarsh on 2023-01-26 21:48_

---

_Comment by @charliermarsh on 2023-01-26 22:03_

I'd recommend disabling `PD` altogether... It's kind of tough to condition these on the presence of `import pandas` statements, since it's not required to import Pandas in a module in order for any given variable to be a DataFrame.


---

_Closed by @charliermarsh on 2023-01-26 22:03_

---

_Referenced in [astral-sh/ruff#2480](../../astral-sh/ruff/issues/2480.md) on 2023-02-02 20:54_

---

_Referenced in [astral-sh/ruff#2741](../../astral-sh/ruff/pulls/2741.md) on 2023-02-10 22:07_

---

_Comment by @mmdanziger on 2023-09-18 09:09_

Ran into this problem with [`torch.max`]() when you do multi-dimensional max you get a namedtuple with attrs `indices` and `values` as in the example code:

```
>>> a = torch.randn(4, 4)
>>> a
tensor([[-1.2360, -0.2942, -0.1222,  0.8475],
        [ 1.1949, -1.1127, -2.2379, -0.6702],
        [ 1.5717, -0.9207,  0.1297, -1.8768],
        [-0.6172,  1.0036, -0.6060, -0.2432]])
>>> torch.max(a, 1)
torch.return_types.max(values=tensor([0.8475, 1.1949, 1.5717, 1.0036]), indices=tensor([3, 0, 0, 1]))
```

In our code-base, this was picked up by `PD011`. Perhaps there can be logic for cases where the use of `.values` are not pandas related, such as this.

---
