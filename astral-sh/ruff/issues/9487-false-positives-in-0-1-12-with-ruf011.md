```yaml
number: 9487
title: False positives in 0.1.12 with RUF011
type: issue
state: closed
author: pablogamboa
labels:
  - bug
assignees: []
created_at: 2024-01-12T07:00:42Z
updated_at: 2024-01-12T19:33:46Z
url: https://github.com/astral-sh/ruff/issues/9487
synced_at: 2026-01-10T11:09:51Z
```

# False positives in 0.1.12 with RUF011

---

_Issue opened by @pablogamboa on 2024-01-12 07:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

ruff: `0.1.12`
invokation: `ruff foobar.py`
settings:
```toml
[tool.ruff]
line-length = 120
select = ["B", "C4", "D", "E", "F", "G", "I", "INP", "ISC", "N", "T20", "PIE", "PT", "RUF", "SIM", "UP", "W"]
ignore = [
  "B008",  # Do not perform function call {name} in argument defaults
  "B017",  # {assertion}({exception}) should be considered evil
  "B904",  # Within an except clause, raise exceptions with raise ... from err or raise ... from None to distinguish the
m from errors in exception handling
  "D100",  # Missing docstring in public module
  "D101",  # Missing docstring in public class
  "D102",  # Missing docstring in public method
  "D103",  # Missing docstring in public function
  "D104",  # Missing docstring in public package
  "D105",  # Missing docstring in magic method
  "D106",  # Missing docstring in public nested class
  "D107",  # Missing docstring in __init__
  "D205",  # 1 blank line required between summary line and description
  "D415",  # First line should end with a period, question mark, or exclamation point
  "E501",  # Line too long
  "ISC001", # Single line implicit string concatenation (disabled as conflicts with ruff's formatter)
  "PT004", # Fixture does not return anything, add leading underscore
  "PT011", # pytest.raises({exception}) is too broad, set the match parameter or use a more specific exception
  "PT018", # Assertion should be broken down into multiple parts
  "PT019", # Fixture {name} without value is injected as parameter, use @pytest.mark.usefixtures instead
  "RUF005", # Consider {expr} instead of concatenation
  "RUF012", # Mutable class attributes should be annotated with `typing.ClassVar`
  "RUF015", # Prefer `next(...)` over single element slice
  "SIM102", # Use a single `if` statement instead of nested `if` statements
  "SIM108", # Use ternary operator instead of `if`-`else`-block
  "SIM114", # Combine `if` branches using logical `or` operator
  "SIM117", # Use a single `with` statement with multiple contexts instead of nested `with` statements
  "N805",   # First argument of a method should be named `self`
]

fixable = ["B", "C4", "D", "E", "F", "I", "N", "PIE", "PT", "RUF", "SIM", "UP"]
unfixable = []
```

snippet:
```python
def _extract_local_id(foo):
    return foo


tokens = {"a": "b"}
tokens = {local_id: token for token in tokens if (local_id := _extract_local_id(token)) is not None}
```

output:
```bash
foobar.py:6:11: RUF011 Dictionary comprehension uses static key: `local_id`
Found 1 error.
```

---

_Label `bug` added by @charliermarsh on 2024-01-12 16:11_

---

_Comment by @charliermarsh on 2024-01-12 16:11_

Oops, thanks -- will fix!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-12 16:25_

---

_Closed by @charliermarsh on 2024-01-12 19:33_

---
