```yaml
number: 18772
title: "[flake8-pytest-style] PT023/PT026 do not check that pytest is pytest"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - rule
assignees: []
created_at: 2025-06-18T23:32:20Z
updated_at: 2025-06-19T09:49:35Z
url: https://github.com/astral-sh/ruff/issues/18772
synced_at: 2026-01-10T11:09:58Z
```

# [flake8-pytest-style] PT023/PT026 do not check that pytest is pytest

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 23:32_

### Summary

From what I can tell,  all `PT` rules that use `pytest.<something>` will make sure `pytest` is from `import pytest` before running, besides `PT023` and `PT026` [playground](https://play.ruff.rs/d1a628c5-32b2-4c76-9d65-95f94109cc53)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
#import pytest

@pytest.mark.parametrize()
def test_bar(): ...

@pytest.fixture()
def my_fixture(): ...


@pytest.fixture(scope="function")
def my_fixture(): ...

# single parameter, always expecting string
@pytest.mark.parametrize(("param",), [1, 2, 3])
def test_foo(param): ...


# multiple parameters, expecting tuple
@pytest.mark.parametrize(["param1", "param2"], [(1, 2), (3, 4)])
def test_bar(param1, param2): ...


# multiple parameters, expecting tuple
@pytest.mark.parametrize("param1,param2", [(1, 2), (3, 4)])
def test_baz(param1, param2): ...

@pytest.mark.parametrize("param", [1, 2, 3])
def test_foo(param): ...


@pytest.mark.parametrize(("param1", "param2"), [(1, 2), (3, 4)])
def test_bar(param1, param2): ...

@pytest.mark.parametrize(
    ("param1", "param2"),
    [
        (1, 2),
        (1, 2),
    ],
)
def test_foo(param1, param2): ...

def test_foo():
    if some_condition:
        pytest.fail("some_condition was True")
    ...

@pytest.mark.asyncio
@pytest.fixture()
async def my_fixture():
    return 0

@pytest.fixture()
def a():
    pass


@pytest.mark.usefixtures("a")
@pytest.fixture()
def b(a):
    pass

@pytest.mark.usefixtures
def test_something(): ...

def test_foo():
    with pytest.warns():
        do_something()

def test_foo():
    with pytest.warns(RuntimeWarning):
        ...

    # empty string is also an error
    with pytest.warns(RuntimeWarning, match=""):
        ...
def test_foo_warns():
    with pytest.warns(Warning):
        setup()  # False negative if setup triggers a warning but foo does not.
        foo()
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select PT
```
```
issue.py:3:1: PT023 [*] Use `@pytest.mark.parametrize` over `@pytest.mark.parametrize()`
  |
1 | #import pytest
2 |
3 | @pytest.mark.parametrize()
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^ PT023
4 | def test_bar(): ...
  |
  = help: Remove parentheses

issue.py:63:1: PT026 Useless `pytest.mark.usefixtures` without parameters
   |
61 |     pass
62 |
63 | @pytest.mark.usefixtures
   | ^^^^^^^^^^^^^^^^^^^^^^^^ PT026
64 | def test_something(): ...
   |
   = help: Remove `usefixtures` decorator or pass parameters

Found 2 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-18 23:54_

---

_Label `bug` added by @ntBre on 2025-06-19 00:04_

---

_Label `rule` added by @ntBre on 2025-06-19 00:04_

---

_Closed by @MichaReiser on 2025-06-19 09:49_

---
