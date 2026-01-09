---
number: 12220
title: Conflict between check and format with D203
type: issue
state: closed
author: arylix
labels:
  - incompatibility
assignees: []
created_at: 2024-07-06T19:20:10Z
updated_at: 2024-07-08T13:13:38Z
url: https://github.com/astral-sh/ruff/issues/12220
synced_at: 2026-01-07T13:12:15-06:00
---

# Conflict between check and format with D203

---

_Issue opened by @arylix on 2024-07-06 19:20_

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

```py

class Test:
    """
    Test class docstring.
    """
```
```toml
lint.select = ["D"]
lint.ignore = ["D100", "D200", "D212", "D211"]
```

When I run `ruff check test.py --config ruff.toml` I get:

```
test.py:2:5: D203 [*] 1 blank line required before class docstring
  |
1 |   class Test:
2 |       """
  |  _____^
3 | |     Test class docstring.
4 | |     """
  | |_______^ D203
  |
  = help: Insert 1 blank line before class docstring

Found 1 error.
```

With `--fix` Ruff changes the file to:

```py
class Test:

    """
    Test class docstring.
    """
```

When I now run `ruff format test.py --config ruff.toml` it redoes the changes and removes the blank line:
```py
class Test:
    """
    Test class docstring.
    """
```

Ruff version: 0.5.1



---

_Comment by @MichaReiser on 2024-07-08 08:07_

Thanks for reporting this. It's interesting that this only comes up now :) 

I don't think there's a way to make this rule formatter compatible. But we should warn when running `ruff format` and the rule is enabled.

---

_Label `incompatibility` added by @MichaReiser on 2024-07-08 08:07_

---

_Referenced in [astral-sh/ruff#12238](../../astral-sh/ruff/pulls/12238.md) on 2024-07-08 08:17_

---

_Comment by @arylix on 2024-07-08 10:23_

So it will definitely not be possible to use D203 with the formatter?

---

_Closed by @MichaReiser on 2024-07-08 13:12_

---

_Comment by @MichaReiser on 2024-07-08 13:13_

No, I'm sorry. The formatter is designed to be compatible with black and follows its guidelines. We may want to deviate from their style guide in the future or allow more customizability, but that's something we yet have to decide on.

---
