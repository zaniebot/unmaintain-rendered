```yaml
number: 1740
title: Autofix PT004, PT005, PT024, and PT025
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: more-autofix-PT009
created_at: 2023-01-09T03:11:40Z
updated_at: 2023-01-09T20:05:37Z
url: https://github.com/astral-sh/ruff/pull/1740
synced_at: 2026-01-12T05:36:32Z
```

# Autofix PT004, PT005, PT024, and PT025

---

_Pull request opened by @harupy on 2023-01-09 03:11_

Autofix PT004, PT005, PT024, and PT025.

---

_Merged by @charliermarsh on 2023-01-09 03:41_

---

_Closed by @charliermarsh on 2023-01-09 03:41_

---

_Comment by @edgarrmondragon on 2023-01-09 20:05_

@harupy autofixing `PT004` and `PT005` may be problematic because fixture names are used to perform dependency injection in pytest test functions.

Consider:

```python
import pytest

@pytest.fixture()
def _my_fixture():
    return 1


def test_something(_my_fixture):
    assert _my_fixture == 1
```

`pytest` works fine:

```console
$ pytest test.py
==================================================================================================================== test session starts =====================================================================================================================
platform darwin -- Python 3.10.9, pytest-7.2.0, pluggy-1.0.0
rootdir: /path/to/project
collected 1 item                                                                                                                                                                                                                                             

test.py .                                                                                                                                                                                                                                             [100%]

===================================================================================================================== 1 passed in 0.01s ======================================================================================================================
```

but after fixing

```console
$ ruff --select PT005 test.py
test.py:4:1: PT005 Fixture `_my_fixture` returns a value, remove leading underscore
Found 1 error(s).
1 potentially fixable with the --fix option.

$ ruff --fix --select PT005 test.py
Found 1 error(s) (1 fixed, 0 remaining).
```

the fixture name no longer works:

```console
$ pytest test.py
==================================================================================================================== test session starts =====================================================================================================================
platform darwin -- Python 3.10.9, pytest-7.2.0, pluggy-1.0.0
rootdir: /path/to/project
collected 1 item                                                                                                                                                                                                                                             

test.py E                                                                                                                                                                                                                                             [100%]

=========================================================================================================================== ERRORS ===========================================================================================================================
______________________________________________________________________________________________________________ ERROR at setup of test_something ______________________________________________________________________________________________________________
file /path/to/project/test.py, line 8
  def test_something(_my_fixture):
E       fixture '_my_fixture' not found
>       available fixtures: cache, capfd, capfdbinary, caplog, capsys, capsysbinary, doctest_namespace, monkeypatch, my_fixture, pytestconfig, record_property, record_testsuite_property, record_xml_attribute, recwarn, tmp_path, tmp_path_factory, tmpdir, tmpdir_factory
>       use 'pytest --fixtures [testpath]' for help on them.

/path/to/project/test.py:8
================================================================================================================== short test summary info ===================================================================================================================
ERROR test.py::test_something
====================================================================================================================== 1 error in 0.01s ======================================================================================================================
```

---
