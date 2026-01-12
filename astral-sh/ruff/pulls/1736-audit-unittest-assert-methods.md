```yaml
number: 1736
title: Audit unittest assert methods
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: audit-unittest-assert-methods
created_at: 2023-01-08T05:21:04Z
updated_at: 2023-01-08T21:21:35Z
url: https://github.com/astral-sh/ruff/pull/1736
synced_at: 2026-01-12T05:36:32Z
```

# Audit unittest assert methods

---

_Pull request opened by @harupy on 2023-01-08 05:21_

I ran the following code in Python 3.10 to automatically generate a list of enums.

```python
import unittest

print(
    ",\n".join(
        sorted(
            m.removeprefix("assert") if m != "assert_" else "Underscore"
            for m in dir(unittest.TestCase)
            if m.startswith("assert")
        )
    )
)
```

---

_@harupy reviewed on 2023-01-08 05:22_

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:31 on 2023-01-08 05:22_

Maybe we should remove `Logs` and `NoLogs` because they can't be simply replaced with `assert ...` or pytest functions.

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:31 on 2023-01-08 05:23_

~The same applies to `Warns` and `Raises` (and their variants).~

---

_@harupy reviewed on 2023-01-08 05:23_

---

_@harupy reviewed on 2023-01-08 05:27_

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:31 on 2023-01-08 05:27_

For `Warns` and `Raises` (and their variants), we could say `use pytest.raises|pytest.warns` instead of `use assert ...`.

---

_@harupy reviewed on 2023-01-08 05:31_

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:18 on 2023-01-08 05:31_

`DictContainsSubset` exists, but is deprecated and hidden in [the unittest doc](https://docs.python.org/3/library/unittest.html#module-unittest):

https://github.com/python/cpython/blob/4b4e6da7b53f9eb2d3654d321f6acfbd599bd990/Lib/unittest/case.py#L1174

---

_Merged by @charliermarsh on 2023-01-08 21:21_

---

_Closed by @charliermarsh on 2023-01-08 21:21_

---
