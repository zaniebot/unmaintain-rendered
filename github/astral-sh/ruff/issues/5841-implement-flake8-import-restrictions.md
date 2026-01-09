---
number: 5841
title: Implement flake8-import-restrictions
type: issue
state: open
author: justinchuby
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-07-17T21:03:06Z
updated_at: 2025-05-28T13:04:55Z
url: https://github.com/astral-sh/ruff/issues/5841
synced_at: 2026-01-07T13:12:15-06:00
---

# Implement flake8-import-restrictions

---

_Issue opened by @justinchuby on 2023-07-17 21:03_

When following the Google style guide and to avoid cyclic imports, we would really like to see the rule from https://github.com/atollk/flake8-import-restrictions#imr241

(import modules only and not the functions and classes from the modules)

> IMR241
> When using the from syntax, only submodules are imported, not module elements.

```
# Bad
from os.path import join

# Good
from os import path
```

---

_Comment by @justinchuby on 2023-07-17 21:04_

So it’s a duplicate of https://github.com/astral-sh/ruff/issues/3045, but I found flake8-import-restrictions which has a good collection of related rules. 

---

_Comment by @edgarrmondragon on 2023-07-17 21:47_

It's not exactly equivalent, but the [`banned-from`](https://beta.ruff.rs/docs/settings/#flake8-import-conventions-banned-from) setting can get a similar violation with this config:

```toml
# ruff.toml
[flake8-import-conventions]
banned-from = ["os.path"]
```

```console
$ ruff check --select ICN myfile.py
myfile.py:2:1: ICN003 Members of `os.path` should not be imported explicitly
Found 1 error.
```

---

_Comment by @justinchuby on 2023-07-17 21:49_

True but it is impossible to configure the list for all possible packages. It would be good to recognize an imported entity not being a module and error on that.

---

_Label `rule` added by @charliermarsh on 2023-07-19 01:20_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-19 01:20_

---

_Comment by @charliermarsh on 2023-07-19 01:21_

We would need to integrate our Python module resolver to support this (doable).

---

_Comment by @QuentinSoubeyranAqemia on 2023-12-11 13:37_

I would also be interested in a rule to disallow importing members, and only allow importing module (or packages, but they are module too technically speaking), per the Google Code Style.
Having a config option for exceptions (like the `typing`, `typing_extensions` and `collections.abc` defined in the google style guide) would be most useful too.

I'm curious what is this "Python module resolver"? Is this an upcoming feature? Some internal that would need to be exposed to rules? Something devs are thinking of doing?

---

_Comment by @Greesb on 2025-05-28 13:04_

Any news on this feature ?

---

_Referenced in [astral-sh/ruff#21118](../../astral-sh/ruff/pulls/21118.md) on 2025-10-29 15:08_

---
