```yaml
number: 714
title: "Should `S101` be ignored in a test file?"
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2022-11-13T00:55:28Z
updated_at: 2022-11-13T01:59:57Z
url: https://github.com/astral-sh/ruff/issues/714
synced_at: 2026-01-10T15:56:05Z
```

# Should `S101` be ignored in a test file?

---

_Issue opened by @harupy on 2022-11-13 00:55_

Should `S101` (assert detection) be ignored in a test file? We do use `assert` in a test file:

```python
# tests/test_foo.py


def test_foo():
    assert foo() == 1
```

---

_Comment by @harupy on 2022-11-13 01:03_

Another assert use case that should be considered valid:

```python
# tests/utils.py: A collection of helper functions for testing

def assert_foo(x, y):
   assert x == y


def assert_bar(x, y):
   assert x != y
```

---

_Comment by @andersk on 2022-11-13 01:30_

It’s hard to imagine a heuristic that would satisfy everyone here. Some people might want to forbid `assert` even in tests, in favor of test-specific methods with more useful failure output like `self.assertEqual(x, y)`.

But better configurability would help. In general, as we add support for more opinionated rules, it does seem inevitable that people will want to be more selective about which files to apply them in. Perhaps we could take some inspiration from the `overrides` configration sections of [ESLint](https://eslint.org/docs/latest/user-guide/configuring/configuration-files#how-do-overrides-work) and [mypy](https://mypy.readthedocs.io/en/stable/config_file.html#using-a-pyproject-toml-file)?

---

_Comment by @harupy on 2022-11-13 01:52_

> But better configurability would help

Agree. It looks like `bandit` provides an option for specifying files where assert detection is disabled: 
https://github.com/PyCQA/bandit/blob/f909a7dea9495517a8550c9b9f6a75bd286ce7e3/bandit/plugins/asserts.py#L20

```
**Config Options:**

You can configure files that skip this check. This is often useful when you
use assert statements in test cases.

.. code-block:: yaml
    assert_used:
      skips: ['*_test.py', '*test_*.py']

```


---

_Comment by @harupy on 2022-11-13 01:59_

Confirmed `--per-file-ignores=tests/*:S101` can disable `S101` in files under `tests`.

---

_Closed by @harupy on 2022-11-13 01:59_

---
