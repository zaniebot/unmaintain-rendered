```yaml
number: 9620
title: Using ruff to format doctests in .rst (and other format) files
type: issue
state: closed
author: jagerber48
labels: []
assignees: []
created_at: 2024-01-23T04:34:16Z
updated_at: 2025-12-10T18:46:46Z
url: https://github.com/astral-sh/ruff/issues/9620
synced_at: 2026-01-12T15:54:49Z
```

# Using ruff to format doctests in .rst (and other format) files

---

_@jagerber48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

#7146 introduces the ability to format code examples within docstrings in `.py` files. In [this message](https://github.com/astral-sh/ruff/issues/7146#issuecomment-1811115865) outlining the proposed strategy it was specifically deemed out of scope to apply formatting to `.rst` or `.md` files

> **Formatting Python code in other formats**
This means you won't be able to run the formatter over a .rst or a .md file
and have it format any Python code blocks in it. This initial implementation
will only support reformatting Python snippets within docstrings inside Python
files. We can consider formatting Python code in other formats as a future
project.

I'm opening up this issue to request such an ability be added to `ruff` it that is not already being considered. Or, if it has been considered, then to discuss the issue or hear why it wouldn't be possible.

---

_Comment by @zanieb on 2024-01-23 05:21_

Hi! This looks like a duplicate of https://github.com/astral-sh/ruff/issues/8237 — let us know if that doesn't cover your use case. In short, we want to do it we just haven't started yet :)

---

_Closed by @zanieb on 2024-01-23 05:21_

---

_Comment by @jagerber48 on 2024-01-23 05:36_

@zanieb yes, duplicate. I tried looking but didn't find it. Thank you!

---

_Comment by @ulgens on 2025-12-10 18:44_

@zanieb Any chance to reopen the issue and close it as a duplicate, with a link to #8237 ? The current resolution seems misleading.

---

_Closed by @AlexWaygood on 2025-12-10 18:46_

---
