---
number: 6412
title: "[Autofix error] Failed to format"
type: issue
state: closed
author: gaborbernat
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-08-08T01:43:14Z
updated_at: 2023-08-08T03:44:19Z
url: https://github.com/astral-sh/ruff/issues/6412
synced_at: 2026-01-07T13:12:15-06:00
---

# [Autofix error] Failed to format

---

_Issue opened by @gaborbernat on 2023-08-08 01:43_

```python
from __future__ import annotations

from json import JSONDecodeError


def _as():
    try:
        pass
    except (JSONDecodeError, JSONDecodeError, TypeError):
        raise ValueError("")

```

```bash
❯ ruff check --fix magic.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `magic.py`, the rule codes B014, EM101, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

magic.py:1:1: D100 Missing docstring in public module
magic.py:6:5: ANN202 Missing return type annotation for private function `_as`
magic.py:9:12: B014 Exception handler with duplicate exception: `JSONDecodeError`
magic.py:10:9: B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
magic.py:10:9: TRY200 Use `raise from` to specify exception cause
magic.py:10:26: EM101 Exception must not use a string literal, assign to variable first
Found 6 errors.
```

```bash
❯ ruff --version
ruff 0.0.282
```

```toml
[tool.ruff]
select = ["ALL"]
line-length = 120
target-version = "py311"
isort = {known-first-party = ["events_gateway", "tests"], required-imports = ["from __future__ import annotations"]}
ignore = [
    "ANN101", # Missing type annotation for `self` in method
    "ANN401", # Dynamically typed expressions (typing.Any) are disallowed
    "D203",  # `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible
    "D212",  # `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible
    "ANN102", #  Missing type annotation for `cls` in classmethod
]
```

---

_Comment by @dhruvmanila on 2023-08-08 02:16_

Hey, thanks for reporting the bug. It's a problem in `B014` autofix which is removing the parentheses:

```python
from __future__ import annotations

from json import JSONDecodeError


def _as():
    try:
        pass
    except JSONDecodeError, TypeError:
        raise ValueError("")
```

---

_Label `bug` added by @dhruvmanila on 2023-08-08 02:16_

---

_Label `autofix` added by @dhruvmanila on 2023-08-08 02:16_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-08-08 02:23_

---

_Comment by @dhruvmanila on 2023-08-08 02:53_

Actually, I'm not sure what the correct solution is but I'm going to put up my findings here. It seems that the generator isn't adding parentheses around tuple expression because we're only passing the tuple expression without any surrounding context. This means that the current level of generation is not going to be greater than the tuple precedence and thus the parentheses aren't added. Without the context, the generator always defaults to not adding the parentheses while in the context of an `except` block the parentheses should always be added if there are more than 1.

We could either just create the autofix manually without the generator or allow passing in the context in some way to the generator.

---

_Referenced in [astral-sh/ruff#6415](../../astral-sh/ruff/pulls/6415.md) on 2023-08-08 03:16_

---

_Closed by @dhruvmanila on 2023-08-08 03:44_

---
