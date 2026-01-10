```yaml
number: 8750
title: __main__ is considered third party by isort
type: issue
state: closed
author: Numerlor
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-11-18T02:34:34Z
updated_at: 2023-11-21T11:52:30Z
url: https://github.com/astral-sh/ruff/issues/8750
synced_at: 2026-01-10T11:09:51Z
```

# __main__ is considered third party by isort

---

_Issue opened by @Numerlor on 2023-11-18 02:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```py
import os

import __main__
import third_party

import first_party

os.a
third_party.a
__main__.a
first_party.a
```
formatting the four imports results in the above order instead of
```py
import os

import third_party

import __main__
import first_party
```

https://play.ruff.rs/e1e21a3a-5567-4c46-9f6d-5ef508551206

---

_Label `bug` added by @charliermarsh on 2023-11-18 02:39_

---

_Comment by @charliermarsh on 2023-11-18 02:39_

It seems that `isort` does the same, but I guess that's also a bug?

---

_Label `good first issue` added by @zanieb on 2023-11-20 16:01_

---

_Comment by @Ezrashaw on 2023-11-20 23:26_

I'd like to work on this. I assume that `__main__` just has to be special-cased?

---

_Comment by @charliermarsh on 2023-11-21 00:23_

That's right -- mostly likely in here: https://github.com/astral-sh/ruff/blob/e30635941140a3ad9418f3b940a8f5507ddb7f1a/crates/ruff_linter/src/rules/isort/categorize.rs#L68

---

_Closed by @charliermarsh on 2023-11-21 11:52_

---
