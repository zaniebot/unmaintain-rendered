```yaml
number: 11254
title: Latest version of ruff (0.4.2) removes blank double blank line that was allowed previously
type: issue
state: open
author: taranlu-houzz
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-05-02T23:21:12Z
updated_at: 2024-08-08T06:01:43Z
url: https://github.com/astral-sh/ruff/issues/11254
synced_at: 2026-01-10T11:09:53Z
```

# Latest version of ruff (0.4.2) removes blank double blank line that was allowed previously

---

_Issue opened by @taranlu-houzz on 2024-05-02 23:21_

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
`ruff --version`: 0.4.2

I recently updated to the latest version of `ruff` and noticed that in specific cases, a double blank line is now getting changed to a single blank line. I strongly prefer to use double blank lines to visually separate some sections of code, and this only seems to happen in one specific scenario so it is kind of driving me crazy. Here is a code snippet that triggers the change:
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


"""Run CLI if package is run directly as a module."""


####################################################################################################
# Imports
####################################################################################################


# ==Package==
from ._private import cli_test_utils


####################################################################################################
# Main
####################################################################################################


cli_test_utils.run_app()
```

When this is formatted using `ruff format <file>` (no other flags needed), it remove the blank line above `# ==Package==`, but leaves all other double blank lines:
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


"""Run CLI if package is run directly as a module."""


####################################################################################################
# Imports
####################################################################################################

# ==Package==
from ._private import cli_test_utils


####################################################################################################
# Main
####################################################################################################


cli_test_utils.run_app()
```

In the past, it did not do this. I'm not sure what the previous working version is, but I may try a few previous versions to see if I can narrow it down.

Update: The last version where it didn't delete the blank line was v0.2.2. The next version, v0.3.0 exhibits the new behavior.

---

_Comment by @zanieb on 2024-05-03 00:34_

Hi! It sounds like this was introduced in our stable style mentioned in the [v0.3.0 changelog](https://github.com/astral-sh/ruff/releases/tag/v0.3.0). You can see more details about the stable style at https://github.com/astral-sh/ruff/issues/8678

However, it looks like this is _not_ a part of Black's stable style e.g. I tested with `black==24.4.2` and this line is not removed. I wonder if our behavior is intentional.

cc @MichaReiser 

---

_Label `formatter` added by @zanieb on 2024-05-03 00:34_

---

_Comment by @MichaReiser on 2024-05-03 06:29_

Yeah, I consider this a bug. The two blank lines should be preserved. I checked out v0.2.2 and narrowed it down that this is due to the preview changes in https://github.com/astral-sh/ruff/pull/8283/files. Although I'm not sure what's  the cause.

---

_Label `bug` added by @MichaReiser on 2024-05-03 06:29_

---

_Comment by @MichaReiser on 2024-05-03 06:50_

Hehe, okay, this one is kind of funny, and it's less clear what the intended behavior is. Looking at Black's tests, they are fairly explicit that there should always only be one blank line after a module-level docstring, but that's not what we're seeing in case there's a comment after a module-level docstring. 

However, that would only mean that there are two bugs:

* It should only preserve one blank line between the module docstring and the comment
* It should preserve two blank lines between the import section and the import comment.

Note: We should not preserve two blank lines in stub files

---

_Comment by @taranlu-houzz on 2024-08-06 23:48_

@zanieb @MichaReiser Hello, just checking to see what the status is on this.

---

_Comment by @MichaReiser on 2024-08-08 06:01_

Hi @taranlu-houzz Sorry, I haven't had time to look into this more. 

---
