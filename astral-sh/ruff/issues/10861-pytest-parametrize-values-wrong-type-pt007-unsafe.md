```yaml
number: 10861
title: "`pytest-parametrize-values-wrong-type` (`PT007` ) unsafe fix fails if there is a list of only one item"
type: issue
state: closed
author: nyoungstudios
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-04-10T16:58:31Z
updated_at: 2024-04-10T18:20:10Z
url: https://github.com/astral-sh/ruff/issues/10861
synced_at: 2026-01-12T15:54:50Z
```

# `pytest-parametrize-values-wrong-type` (`PT007` ) unsafe fix fails if there is a list of only one item

---

_@nyoungstudios_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Here is a minimal `.ruff.toml` file using `0.3.5`:
```toml
line-length = 120

target-version = "py38"

lint.extend-select = [
    "W",  # whitespace
    "I",  # imports

    # pytest
    "PT001",  # pytest-fixture-incorrect-parentheses-style
    "PT006",  # pytest-parametrize-names-wrong-type
    "PT007",  # pytest-parametrize-values-wrong-type
    "PT025",  # pytest-erroneous-use-fixtures-on-fixture
    "PT026",  # pytest-use-fixtures-without-parameters
]

[ lint.isort ]

known-first-party = ["module"]

[ lint.flake8-pytest-style ]

fixture-parentheses = false
parametrize-names-type = "csv"
parametrize-values-row-type = "tuple"
parametrize-values-type = "tuple"
```

Here is the code snippet
```python
import pytest
from pytest_mock import MockFixture

from module.sub import (
    get_value,
)


@pytest.fixture
def mock_check_output(mocker: MockFixture, output: bytes):
    return mocker.patch("subprocess.check_output", return_value=output)


@pytest.mark.parametrize(
    "output, expected_result",
    [(b"apple", "apple")],
)
def test_get_value(mock_check_output, expected_result):
    assert get_value() == expected_result
    mock_check_output.assert_called_once()
```

Running this command
```bash
ruff check path/to/file.py --fix --unsafe-fixes
```
outputs this error
```
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `path/to/file.py`, the rule codes PT007, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

path/to/file.py:16:5: PT007 Wrong values type in `@pytest.mark.parametrize` expected `tuple` of `tuple`
Found 1 error.
[*] 1 fixable with the --fix option.
```

However, changing the pytest parameterize code block to:
```python
@pytest.mark.parametrize(
    "output, expected_result",
    [(b"apple", "apple"), (b"banana", "banana")],
)
```
allows the ruff command to run successfully. And successfully changes the parameterize code block to:
```python
@pytest.mark.parametrize(
    "output, expected_result",
    ((b"apple", "apple"), (b"banana", "banana")),
)
```

---

_Comment by @charliermarsh on 2024-04-10 17:02_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-04-10 17:02_

---

_Label `fixes` added by @charliermarsh on 2024-04-10 17:02_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 17:56_

---

_Closed by @charliermarsh on 2024-04-10 18:20_

---
