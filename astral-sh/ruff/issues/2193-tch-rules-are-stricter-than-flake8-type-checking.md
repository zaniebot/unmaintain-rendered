```yaml
number: 2193
title: TCH rules are stricter than flake8-type-checking, but not necessarily better
type: issue
state: closed
author: GabDug
labels:
  - bug
assignees: []
created_at: 2023-01-26T13:58:25Z
updated_at: 2023-01-26T21:04:23Z
url: https://github.com/astral-sh/ruff/issues/2193
synced_at: 2026-01-10T11:09:45Z
```

# TCH rules are stricter than flake8-type-checking, but not necessarily better

---

_Issue opened by @GabDug on 2023-01-26 13:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

TCH rules currently produce much more warnings than flake8-type-checking, but may be less relevant.

Consider this snippet:
```python
from pkg import A,B

def test(value: A):
   return B()
```
With ruff, you'll get a warning. However, the flake8 plugin will not trigger: adding one of the two imports in the TYPE_CHECKING block would not prevent the module from being imported at runtime, at the extra cost of 3 more lines and splitted imports.

The same applies to TCH003: import from `typing` don't benefit much, as we already import it for `TYPE_CHECKING`
```python
from typing import TYPE_CHECKING, Any

if TYPE_CHECKING:
   ...

a: Any = "str"
```

I feel like the having flake8 way would be beneficial, either by default or with an option flag.

Anyway, thanks to all the ruff contributors, this is an incredible tool!

Tested on ruff 0.0.235


---

_Comment by @GabDug on 2023-01-26 14:05_

I'm realising my comment may not be very clear:

TLDR: TCH warnings for types imported outside of TYPE_CHECKING should probably be ignored (by default or opt-in) if there is another import from the same module that is required out of type checking, as it's more verbose with no changes to imported modules at runtime.

---

_Comment by @charliermarsh on 2023-01-26 14:08_

I agree with this! Would be a good change. It may not've come up with the existing flake8-type-checking test suite (which I ported over :))

---

_Label `bug` added by @charliermarsh on 2023-01-26 14:08_

---

_Comment by @charliermarsh on 2023-01-26 14:08_

Gonna call this a bug since it's a clear deviation from "upstream".

---

_Comment by @GabDug on 2023-01-26 14:21_

@charliermarsh it's indeed a bug, as it is different from the plugin default configuration.

However, it looks like there are options to change these behaviours in the original plugin:
https://github.com/snok/flake8-type-checking#strict

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-26 14:49_

---

_Comment by @charliermarsh on 2023-01-26 14:50_

Part of #2195, but I'll leave this open too. I'll do `strict` first.

---

_Closed by @charliermarsh on 2023-01-26 21:04_

---
