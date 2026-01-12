```yaml
number: 76
title: Consider using hypothesmith
type: issue
state: closed
author: sobolevn
labels:
  - internal
assignees: []
created_at: 2022-09-01T16:35:51Z
updated_at: 2023-05-10T21:11:00Z
url: https://github.com/astral-sh/ruff/issues/76
synced_at: 2026-01-12T15:54:40Z
```

# Consider using hypothesmith

---

_@sobolevn_

Consider using https://pypi.org/project/hypothesmith/
It generates lots of python programms to run your tool on.

It really helps finding ones that crash.
Example: https://github.com/wemake-services/wemake-python-styleguide/blob/master/tests/test_checker/test_hypothesis.py

---

_Label `enhancement` added by @charliermarsh on 2022-09-01 16:44_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:21_

---

_Label `internal` added by @charliermarsh on 2022-12-31 18:21_

---

_Comment by @charliermarsh on 2023-05-10 21:10_

We've been getting good panic coverage via `cargo-fuzz` so going to close for now.

---

_Closed by @charliermarsh on 2023-05-10 21:10_

---
