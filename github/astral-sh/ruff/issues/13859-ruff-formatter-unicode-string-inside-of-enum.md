---
number: 13859
title: Ruff formatter - unicode string inside of Enum object always lowercasing characters
type: issue
state: closed
author: DashKenny
labels:
  - question
  - formatter
assignees: []
created_at: 2024-10-21T14:25:51Z
updated_at: 2024-10-21T15:05:32Z
url: https://github.com/astral-sh/ruff/issues/13859
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff formatter - unicode string inside of Enum object always lowercasing characters

---

_Issue opened by @DashKenny on 2024-10-21 14:25_

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

Hi,

I've been trying to upgrade ruff and use the formatter, so we can remove black. I have noticed this diff when using the formatter:

<img width="227" alt="image" src="https://github.com/user-attachments/assets/4a526853-5cca-4b50-9ca1-c858c9a60081">

This enum is used to validate codes from an external API, I would rather not try and handle lowercasing that data before accessing this enum. I cannot seem to find a rule to turn off this behaviour, does this not exist or am I missing something? Nothing interesting popped out to me when running `ruff format --verbose <path>`

Ruff version: `0.7.0`

Code snippet:

```
from enum import Enum


class TestEnum(Enum):
    ANGRY = '\u1F621'
    SAD = '\u1F622'
```

Ruff pyproject settings:
```
[tool.ruff.lint]
select = ["E", "F", "I", "Q", "N"]
ignore = []

[tool.ruff.lint.flake8-quotes]
inline-quotes = "single"

[tool.ruff.format]
quote-style = "single"
```

Command invoked: 
`ruff format <path>` (Doesn't matter if it's only the file, or the whole repo)

---

_Label `question` added by @MichaReiser on 2024-10-21 14:31_

---

_Label `formatter` added by @MichaReiser on 2024-10-21 14:31_

---

_Comment by @MichaReiser on 2024-10-21 14:32_

hi @DashKenny 

The casing of the hex-values in unicode escapes doesn't change the value at runtime:

```
>>> '\u1F621' == '\u1f621'
True
```
It's just a different way to format the same data.

---

_Comment by @DashKenny on 2024-10-21 15:05_

Thanks for the quick reply @MichaReiser and pointing that out. That makes sense.

---

_Closed by @DashKenny on 2024-10-21 15:05_

---

_Referenced in [psf/black#4501](../../psf/black/issues/4501.md) on 2024-10-23 13:50_

---

_Referenced in [psf/black#4522](../../psf/black/issues/4522.md) on 2024-12-04 07:22_

---

_Referenced in [fonttools/fonttools#3881](../../fonttools/fonttools/pulls/3881.md) on 2025-07-04 11:06_

---
