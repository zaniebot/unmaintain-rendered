```yaml
number: 12270
title: Cannot catch double import in TYPE_CHECKING block
type: issue
state: open
author: beskep
labels:
  - question
assignees: []
created_at: 2024-07-10T08:26:18Z
updated_at: 2024-07-13T00:53:07Z
url: https://github.com/astral-sh/ruff/issues/12270
synced_at: 2026-01-10T11:09:54Z
```

# Cannot catch double import in TYPE_CHECKING block

---

_Issue opened by @beskep on 2024-07-10 08:26_

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

command: `ruff check --select TCH,I,F`
version: 0.5.1

code: 
```python
from decimal import Decimal  # I001: import block un-sorted
from decimal import Decimal  # F811: redefinition
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from decimal import Decimal  # reimport, but no error


def fn():
    value: Decimal  # noqa: F842
```

I often import the same bunch of modules by snippet after TCH creates TYPE_CHECKING block.
ruff --fix deletes the second line above and keeps the first line.


---

_Comment by @charliermarsh on 2024-07-12 12:39_

Just to be clear, which line do you think should be flagged as an error, and which should be removed? (And similarly, what if `Decimal` is used outside of a type annotation -- what would you expect?)

---

_Label `question` added by @charliermarsh on 2024-07-12 12:39_

---

_Comment by @beskep on 2024-07-13 00:53_

Removing the first and second lines would be perfect.

Or maybe flag later imports (lines 2 and 6) and remove line 2, 5 and 6.
Then the `TCH` will handle `TYPE_CHECKING` block.

---
