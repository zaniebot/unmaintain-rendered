```yaml
number: 4228
title: Skip __debuggerskip__ variable
type: issue
state: closed
author: propertone
labels:
  - good first issue
assignees: []
created_at: 2023-05-04T19:26:51Z
updated_at: 2023-05-08T11:56:19Z
url: https://github.com/astral-sh/ruff/issues/4228
synced_at: 2026-01-10T11:09:47Z
```

# Skip __debuggerskip__ variable

---

_Issue opened by @propertone on 2023-05-04 19:26_

[IPython 7.29](https://ipython.readthedocs.io/en/7.31.0/whatsnew/version7.html#ipython-7-29) added a feature to support a `__debuggerskip__` variable which give Pdb the ability to skip code in decorators. Similar to `__tracebackhide__`, can ruff be updated to allow the use of `__debuggerskip__` as an unused variable by default for rule F841? These are white listed [here](https://github.com/charliermarsh/ruff/blob/b71cc3789fbb91ede0fcbe2d09479feb82558d84/crates/ruff/src/rules/pyflakes/rules/unused_variable.rs#L327).

---

_Comment by @charliermarsh on 2023-05-04 19:28_

Sure thing, easy enough.

---

_Label `good first issue` added by @charliermarsh on 2023-05-04 19:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-04 19:38_

---

_Closed by @charliermarsh on 2023-05-04 19:45_

---

_Comment by @propertone on 2023-05-08 11:56_

Thanks for the quick fix!

---
