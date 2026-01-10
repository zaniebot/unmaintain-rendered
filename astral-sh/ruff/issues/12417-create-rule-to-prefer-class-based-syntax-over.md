```yaml
number: 12417
title: Create rule to prefer class based syntax over functional syntax for Enums
type: issue
state: open
author: mezuzza
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-07-20T06:10:55Z
updated_at: 2025-03-11T08:47:46Z
url: https://github.com/astral-sh/ruff/issues/12417
synced_at: 2026-01-10T11:09:54Z
```

# Create rule to prefer class based syntax over functional syntax for Enums

---

_Issue opened by @mezuzza on 2024-07-20 06:10_

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

There are a number of cases where you have a choice between class based syntax and functional syntax. The specific case I'm considering is Enum - though I do vaguely recall that there are other cases.

For example, you defined an enum using either of these methods:

```python
class Color(Enum):
    RED = auto()
    YELLOW = auto()
    GREEN = auto()
```

```python
Color = Enum(
    "Color",
    [
        "RED",
        "YELLOW",
        "GREEN",
    ],
)
```

What you prefer is a matter of taste (though honestly, if you prefer functional style, you're a little crazy) - however, I'd like to make sure we prefer the former throughout our codebase at work. This lint could obviously warn in either direction.

I'm a little surprised this suggestion didn't exist before tbh, so feel free to point me to the old ticket if it exists. I did search for "enums" and "functional" to try to find this issue, but I couldn't find it (I also tried a few others queries, but you get the idea).

---

_Label `rule` added by @charliermarsh on 2024-07-20 15:45_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-11 08:47_

---
