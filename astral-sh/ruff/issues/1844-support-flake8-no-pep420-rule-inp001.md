```yaml
number: 1844
title: Support flake8-no-pep420 rule INP001
type: issue
state: closed
author: nefrob
labels:
  - good first issue
  - plugin
assignees: []
created_at: 2023-01-13T01:53:33Z
updated_at: 2023-01-18T03:10:33Z
url: https://github.com/astral-sh/ruff/issues/1844
synced_at: 2026-01-10T11:09:44Z
```

# Support flake8-no-pep420 rule INP001

---

_Issue opened by @nefrob on 2023-01-13 01:53_

PyPI link: https://pypi.org/project/flake8-no-pep420/.

Checks for the existence of an `__init__.py` file in each directory of a python project. Looked through the checkers and didn't see one for file name or path based checks so not sure if Ruff can easily support this right now. The rule itself is fairly short otherwise ([code](https://github.com/adamchainz/flake8-no-pep420/blob/main/src/flake8_no_pep420/__init__.py)).


---

_Label `plugin` added by @charliermarsh on 2023-01-13 01:55_

---

_Label `good first issue` added by @charliermarsh on 2023-01-13 01:55_

---

_Comment by @charliermarsh on 2023-01-15 07:06_

Yeah we can just add a `filesystem` checker in `src/checkers.rs`. Seems straightforward enough!

---

_Closed by @charliermarsh on 2023-01-18 03:10_

---
