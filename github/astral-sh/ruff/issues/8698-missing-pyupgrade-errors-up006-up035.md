---
number: 8698
title: Missing pyupgrade errors (UP006, UP035)
type: issue
state: closed
author: rademacher-p
labels:
  - fixes
assignees: []
created_at: 2023-11-15T17:29:45Z
updated_at: 2023-11-16T01:53:16Z
url: https://github.com/astral-sh/ruff/issues/8698
synced_at: 2026-01-07T13:12:15-06:00
---

# Missing pyupgrade errors (UP006, UP035)

---

_Issue opened by @rademacher-p on 2023-11-15 17:29_

Hi, I seem to be missing pyupgrade errors for Python 3.11 and Ruff 0.1.5. Reproduce w/ `ruff --isolated --select UP file.py` on a fresh `venv`. This is all from the CLI, no `pyproject.toml`.
```
# file.py
from typing import Type

foo = Type[list]
```

I ❤️ `ruff`, thanks in advance for any help!


---

_Comment by @charliermarsh on 2023-11-15 17:36_

Thank you for the kind words!

I think this is a result of https://github.com/astral-sh/ruff/issues/3215 -- we stopped rewriting annotations that are used at runtime (roughly, on the right-hand side of an expression) because it can change behavior. https://github.com/astral-sh/ruff/issues/4843 has some more information. I think we actually _can_ rewrite these in some but not all scenarios. In other words, our current approach is probably too conservative.

---

_Label `autofix` added by @charliermarsh on 2023-11-15 17:36_

---

_Comment by @charliermarsh on 2023-11-15 18:13_

I think it's always safe to perform these rewrites for UP006 (like `List` to `list`, assuming you're on a supported Python version).

It's probably _not_ always safe to perform these rewrites for UP007 (which converts `Union[int, str]` to `int | str`).

---

_Comment by @NeilGirdhar on 2023-11-15 19:27_

I think you should write this as a type alias:
```python
foo: TypeAlias = Type[list]  # Python < 3.12
type foo = type[list]  # Python >= 3.12
```
And then would it be possible for Ruff to make the transformation?  Are right-hand-sides of type aliases okay?

---

_Comment by @rademacher-p on 2023-11-15 21:35_

Aside from the issues with autofix, is there a reason not to provide the deprecation inspection?

---

_Comment by @charliermarsh on 2023-11-15 23:23_

The only reason is to ensure we're not introducing false positives. So it'd be nice to make sure we have a sense for where this is and isn't safe prior to enabling the check for runtime annotations.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-16 00:52_

---

_Comment by @charliermarsh on 2023-11-16 01:10_

Oh hmm, I'm actually noticing that if your Python version is set correctly, we _do_ flag and fix this: https://play.ruff.rs/526e6c64-b8da-4cd5-8dcd-68ee71a6f886

Do you have your `--target-version` configured to Python 3.11?

---

_Comment by @rademacher-p on 2023-11-16 01:50_

@charliermarsh No, I did not! Should have had that in my config (actually, should just up my `project.requires-python`) 😮 Adding this option gives me the pyupgrade errors - feel free to close, if you're ready 👍 

---

_Comment by @charliermarsh on 2023-11-16 01:53_

Awesome, thanks @rademacher-p! Sorry for the confusion but glad to resolve.

---

_Closed by @charliermarsh on 2023-11-16 01:53_

---
