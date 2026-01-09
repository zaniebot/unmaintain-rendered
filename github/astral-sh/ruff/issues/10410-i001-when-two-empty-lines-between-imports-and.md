---
number: 10410
title: I001 when two empty lines between imports and function
type: issue
state: closed
author: mgzenitech
labels:
  - needs-info
assignees: []
created_at: 2024-03-14T08:25:59Z
updated_at: 2024-05-15T12:41:18Z
url: https://github.com/astral-sh/ruff/issues/10410
synced_at: 2026-01-07T13:12:15-06:00
---

# I001 when two empty lines between imports and function

---

_Issue opened by @mgzenitech on 2024-03-14 08:25_

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

This triggers `I001`
```
#!/usr/bin/env -S poetry run python

from typing import Any
from jsonc_parser.parser import JsoncParser


def from_json(
    source: str,
    _: dict[str, str],
) -> dict[str, Any]:
    """
    Parses a JSONC (JSON with comments) string into a Python dictionary.

    Args:
        source: A string containing JSONC content to be parsed.

    Returns:
        A dictionary representation of the parsed JSONC content.
    """
    return JsoncParser.parse_str(source)
```

However this doesn't (please take notice of empty line between imports and function as that is the only difference):
```
#!/usr/bin/env -S poetry run python

from typing import Any
from jsonc_parser.parser import JsoncParser

def from_json(
    source: str,
    _: dict[str, str],
) -> dict[str, Any]:
    """
    Parses a JSONC (JSON with comments) string into a Python dictionary.

    Args:
        source: A string containing JSONC content to be parsed.

    Returns:
        A dictionary representation of the parsed JSONC content.
    """
    return JsoncParser.parse_str(source)
```

---

_Comment by @charliermarsh on 2024-03-14 15:49_

I'm unable to reproduce this. I see an I001 error in both cases: https://play.ruff.rs/68796056-b9bd-4903-8abf-a6d4cf4ce927.

---

_Label `needs-info` added by @charliermarsh on 2024-03-14 15:49_

---

_Comment by @mgzenitech on 2024-03-14 15:54_

Found the issue...

If I set `lines-after-imports = 1` and run formatter it leaves the linter unhappy :D and since there is no way to control this for formatter I was forced to set it to `lines-after-imports = 2` to be able to use both linter and formatter...

---

_Closed by @mgzenitech on 2024-03-14 15:54_

---

_Comment by @charliermarsh on 2024-03-14 16:00_

Oh right! Thanks for following up.

---

_Comment by @vmartyanov on 2024-05-15 12:41_

Got error "Import block is un-sorted or un-formatted" when just one single empty line is absent. The message is not informative when ruff knows well what is expected. Need more informative messages in this case. And number of empty lines should not be an error

---
