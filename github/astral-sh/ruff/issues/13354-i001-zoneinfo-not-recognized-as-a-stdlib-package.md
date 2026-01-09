---
number: 13354
title: "I001: `zoneinfo` not recognized as a stdlib package for PEP-8 compliant import formatting."
type: issue
state: closed
author: gtkacz
labels:
  - question
  - isort
assignees: []
created_at: 2024-09-14T21:14:16Z
updated_at: 2024-09-15T00:54:54Z
url: https://github.com/astral-sh/ruff/issues/13354
synced_at: 2026-01-07T13:12:15-06:00
---

# I001: `zoneinfo` not recognized as a stdlib package for PEP-8 compliant import formatting.

---

_Issue opened by @gtkacz on 2024-09-14 21:14_

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
Ruff doesn't recognize `zoneinfo` (introduced with PEP-615 at Python 3.9) as a first-party package and formats imports with a way not compliant with PEP-8. More details below:

- Python version: 3.12.0
- Ruff version: 0.6.5

Ruff reformats the following correct PEP-8 compliant import statement:

```python
import zoneinfo
from datetime import date, datetime, time
from typing import TYPE_CHECKING, Optional, Sequence, Tuple
```

To the following incorrect one:

```python
from datetime import date, datetime, time
from typing import TYPE_CHECKING, Optional, Sequence, Tuple

import zoneinfo
```

Based on rule `I001 [*] Import block is un-sorted or un-formatted`.

---

_Referenced in [python-telegram-bot/python-telegram-bot#4326](../../python-telegram-bot/python-telegram-bot/pulls/4326.md) on 2024-09-14 21:15_

---

_Renamed from "I001: `zoneinfo` not recognizes as a stdlib package for PEP-8 import formatting." to "I001: `zoneinfo` not recognized as a stdlib package for PEP-8 compliant import formatting." by @gtkacz on 2024-09-14 21:15_

---

_Comment by @gtkacz on 2024-09-14 21:18_

isort and Black for instance do not currently have these issues.

---

_Comment by @AlexWaygood on 2024-09-14 23:51_

Hiya -- do you have `requires-python` or `tool.ruff.target-version` set in your `pyproject.toml` file? I can repro this in the playground without them set (which leads Ruff to infer `"py38"` as the target version, since that's our current default), but if you set `"py39"` as the target version then this isn't an issue: https://play.ruff.rs/640f351d-0d84-4638-b276-4a6239df7f20

---

_Comment by @gtkacz on 2024-09-14 23:58_

@AlexWaygood hey, thanks for the quick response! This came up as a pre-commit check was failing on https://github.com/python-telegram-bot/python-telegram-bot/pull/4326 which does have a `requires-python = ">=3.8"` on their `pyproject.toml` file. They do plan on dropping support on Python 3.8 soon, so I believe that will fix this, but even then, shouldn't Ruff properly format imports without a `pyproject.toml` file set-up? Thanks for your time! :)

---

_Comment by @AlexWaygood on 2024-09-15 00:02_

Well, the issue here is that we assume you'll only be importing stdlib modules available on all versions of Python you support. If you support Python 3.8, we'll only look at the list of stdlib modules available on Python 3.8 when trying to determine if a module is a stdlib module or a third-party module. So I *think* Ruff's behaviour is correct here :-)

If this is causing issues for you, you might find this setting helpful: https://docs.astral.sh/ruff/settings/#lint_isort_extra-standard-library

---

_Label `question` added by @AlexWaygood on 2024-09-15 00:03_

---

_Label `isort` added by @AlexWaygood on 2024-09-15 00:03_

---

_Comment by @gtkacz on 2024-09-15 00:23_

@AlexWaygood that worked for the pre-commit issue, thanks :)! Still, I think if `pyproject.toml` isn't used at all, the imports should be properly formatted. Maybe include a default value of

```toml
[tool.ruff.lint.isort]
extra-standard-library = ["zoneinfo"]
```

When it's not overridden if `requires-python` isn't passed? Specially since the `isort` hook is properly formatting with the same `requires-python = ">=3.8"` configuration in the PR I mentioned

---

_Comment by @AlexWaygood on 2024-09-15 00:31_

Hmm, I guess I'm not sure why you'd be running two separate import sorters in CI (Ruff and isort)! Just one should probably do the trick 😄

That also honestly sounds like a bug in isort rather than ruff if they're treating `zoneinfo` as a stdlib module on Python versions before it was added to the stdlib.

> Still, I think if `pyproject.toml` isn't used at all, the imports should be properly formatted.

We also support the same configuration options via a `ruff.toml` file, if you don't have a `pyproject.toml` file! Also, if you want to tell ruff not to infer the target version from the `requires-python` field, you can set the `tool.ruff.target-version` option instead :-) https://docs.astral.sh/ruff/settings/#target-version

---

_Comment by @gtkacz on 2024-09-15 00:54_

Don't ask me on this one lol 🤷 thanks for the help anyway! :)

---

_Closed by @gtkacz on 2024-09-15 00:54_

---

_Referenced in [astral-sh/ruff#13446](../../astral-sh/ruff/issues/13446.md) on 2024-09-22 05:07_

---

_Referenced in [astral-sh/ruff#10457](../../astral-sh/ruff/issues/10457.md) on 2024-09-22 05:32_

---
