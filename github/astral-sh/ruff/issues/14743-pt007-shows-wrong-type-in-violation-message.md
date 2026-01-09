---
number: 14743
title: PT007 shows wrong type in violation message
type: issue
state: open
author: ThiefMaster
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-12-02T20:56:51Z
updated_at: 2024-12-04T14:02:46Z
url: https://github.com/astral-sh/ruff/issues/14743
synced_at: 2026-01-07T13:12:16-06:00
---

# PT007 shows wrong type in violation message

---

_Issue opened by @ThiefMaster on 2024-12-02 20:56_

The violation message shows the same type twice, even though one of them should be `list`.

```python
import pytest


@pytest.mark.parametrize('foo', [{'bar': 1}, {'baz': 1}])
def test_test(foo):
    pass
```

```sh
$ ruff version
ruff 0.8.1
```

```py
$ ruff check --isolated --select PT007 --config "lint.flake8-pytest-style.parametrize-values-type = 'tuple'" --preview --no-cache rufftest/ruff_sample.py
rufftest/ruff_sample.py:4:33: PT007 Wrong values type in `@pytest.mark.parametrize` expected `tuple` of `tuple`
  |
4 | @pytest.mark.parametrize('foo', [{'bar': 1}, {'baz': 1}])
  |                                 ^^^^^^^^^^^^^^^^^^^^^^^^ PT007
5 | def test_test(foo):
6 |     pass
  |
  = help: Use `tuple` of `tuple` for parameter values

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

The fix looks fine though so this is just a wrong message:

```py
$ ruff check --isolated --select PT007 --config "lint.flake8-pytest-style.parametrize-values-type = 'tuple'" --preview --no-cache rufftest/ruff_sample.py --diff --unsafe-fixes
--- rufftest/ruff_sample.py
+++ rufftest/ruff_sample.py
@@ -1,6 +1,6 @@
 import pytest


-@pytest.mark.parametrize('foo', [{'bar': 1}, {'baz': 1}])
+@pytest.mark.parametrize('foo', ({'bar': 1}, {'baz': 1}))
 def test_test(foo):
     pass

Would fix 1 error.
```

---

_Comment by @charliermarsh on 2024-12-02 21:32_

I think this is right, but perhaps unclear -- it's saying that it "expected a tuple that contains tuples", i.e., `((`.

---

_Comment by @ThiefMaster on 2024-12-02 21:34_

Oh indeed, I read this "tuple of tuple" as "tuple instead of tuple"...

---

_Comment by @charliermarsh on 2024-12-02 21:35_

Yeah I think we can make it clearer...

---

_Comment by @ThiefMaster on 2024-12-02 21:40_

FWIW the real code where this happened was this:

```py
def setup_fixtures(func):
    """Set up fixtures (utlity decorator)."""
    func = pytest.mark.usefixtures('smtp', 'mock_access_request', 'dummy_access_request')(func)
    return pytest.mark.parametrize('mock_access_request', [{
        'during_registration': True,
        'during_registration_required': True,
        'personal_data': generate_personal_data(),
        'include_accompanying_persons': False
    }, {
        'during_registration': True,
        'during_registration_required': True,
        'personal_data': generate_personal_data(),
        'include_accompanying_persons': True
    }], indirect=True)(func)
```

I guess with `indirect=True` parametrization, but also with any case where there's only a single param, the error message is particularly strange. I haven't tested it, but I guess w/a single param the actual param value inside the outer tuple can be anything (not sure what happens if you use a tuple/list there though).

---

_Label `rule` added by @dhruvmanila on 2024-12-03 04:41_

---

_Label `help wanted` added by @dhruvmanila on 2024-12-03 04:41_

---

_Comment by @harupy on 2024-12-04 14:02_

Can we remove `of {expected_row_type}` for a single param?

```python
$ ruff check --isolated --select PT007 --config "lint.flake8-pytest-style.parametrize-values-type = 'tuple'" --preview --no-cache rufftest/ruff_sample.py
rufftest/ruff_sample.py:4:33: PT007 Wrong values type in `@pytest.mark.parametrize` expected `tuple`

  |
4 | @pytest.mark.parametrize('foo', [{'bar': 1}, {'baz': 1}])
  |                                 ^^^^^^^^^^^^^^^^^^^^^^^^ PT007
5 | def test_test(foo):
6 |     pass
  |
  = help: Use `tuple` of `tuple` for parameter values

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---
