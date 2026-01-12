```yaml
number: 18771
title: "[flake8-pytest-style] PT001/PT023 fix makes syntax error on parenthesized decorator"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-18T21:41:45Z
updated_at: 2025-06-23T11:46:16Z
url: https://github.com/astral-sh/ruff/issues/18771
synced_at: 2026-01-12T15:54:56Z
```

# [flake8-pytest-style] PT001/PT023 fix makes syntax error on parenthesized decorator

---

_@MeGaGiGaGon_

### Summary

As the title says, fixing either of these will cause a syntax error to be made [playground](https://play.ruff.rs/b5bc5fd8-7a46-4cf3-a35c-be62e430b91b)
`PT001`:
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
import pytest

@(pytest.fixture())
def my_fixture(): ...
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select PT --fix
```
```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes PT001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:3:1: PT001 Use `@pytest.fixture` over `@pytest.fixture()`
  |
1 | import pytest
2 |
3 | @(pytest.fixture())
  | ^^^^^^^^^^^^^^^^^^^ PT001
4 | def my_fixture(): ...
  |
  = help: Remove parentheses

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
`PT023`:
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
import pytest

@(pytest.mark.parametrize())
def test_bar(): ...
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select PT --fix
```
```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes PT023, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:3:1: PT023 Use `@pytest.mark.parametrize` over `@pytest.mark.parametrize()`
  |
1 | import pytest
2 |
3 | @(pytest.mark.parametrize())
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PT023
4 | def test_bar(): ...
  |
  = help: Remove parentheses

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
I found this since I wanted to use the parenthesis to put even more comments in to be deleted for #18770

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-19 00:05_

---

_Label `fixes` added by @ntBre on 2025-06-19 00:05_

---

_Label `help wanted` added by @ntBre on 2025-06-19 00:05_

---

_Closed by @MichaReiser on 2025-06-23 11:46_

---
