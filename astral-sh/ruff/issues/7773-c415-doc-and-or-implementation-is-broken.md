---
number: 7773
title: C415 doc and/or implementation is broken
type: issue
state: closed
author: JelleZijlstra
labels:
  - documentation
assignees: []
created_at: 2023-10-03T03:58:03Z
updated_at: 2023-10-03T04:18:51Z
url: https://github.com/astral-sh/ruff/issues/7773
synced_at: 2026-01-10T01:22:47Z
---

# C415 doc and/or implementation is broken

---

_Issue opened by @JelleZijlstra on 2023-10-03 03:58_

The documentation for C415 (https://docs.astral.sh/ruff/rules/unnecessary-subscript-reversal/) states that it makes the following changes:

```
Examples:
reversed(iterable[::-1])
set(iterable[::-1])
sorted(iterable)[::-1]

Use instead:
reversed(iterable)
set(iterable)
sorted(iterable, reverse=True)
```

But unless I'm doing something wrong, it doesn't:

```
$ cat rev.py 
def f(iterable):
    x = reversed(iterable[::-1])
    y = set(iterable[::-1])
    z = sorted(iterable)[::-1]
    return x, y, z
$ python -m ruff check --select=C415 --fix --diff rev.py 
$ python -m ruff --version
ruff 0.0.292
```

Notice no warnings.

The reason I was checking this in the first place was that the first example (changing `reversed(iterable[::-1])` to `reversed(iterable)`) is obviously wrong: if Ruff did that, it would result in the eventual order of the output being the wrong way. Possibly Ruff should output just `iterable` in this case, though it doesn't feel like a common issue.

---

_Comment by @charliermarsh on 2023-10-03 04:03_

Yeah, that's definitely wrong.

---

_Label `documentation` added by @charliermarsh on 2023-10-03 04:03_

---

_Comment by @charliermarsh on 2023-10-03 04:03_

Need to figure out whether it's the docs, implementation, or both. Thanks!

---

_Comment by @JelleZijlstra on 2023-10-03 04:04_

And I'm indeed doing something wrong: if `--fix` is passed, Ruff doesn't print warnings. 

```
$ python -m ruff check rev.py --select=C4
rev.py:2:9: C415 Unnecessary subscript reversal of iterable within `reversed()`
rev.py:3:9: C415 Unnecessary subscript reversal of iterable within `set()`
```

But that still leaves two issues:
* The doc example about reversed() is wrong
* The implementation doesn't complain about the sorted() pattern the doc mentions

---

_Comment by @charliermarsh on 2023-10-03 04:05_

(Ruff does print warnings with `--fix`; I think it's `--diff` that suppresses the warnings, since the purpose of that flag is to show fix changes.)

---

_Referenced in [astral-sh/ruff#7774](../../astral-sh/ruff/pulls/7774.md) on 2023-10-03 04:10_

---

_Closed by @charliermarsh on 2023-10-03 04:18_

---
