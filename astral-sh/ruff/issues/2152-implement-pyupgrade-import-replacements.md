```yaml
number: 2152
title: "Implement `pyupgrade` import replacements"
type: issue
state: closed
author: stinodego
labels:
  - rule
assignees: []
created_at: 2023-01-25T11:23:23Z
updated_at: 2023-01-25T12:27:05Z
url: https://github.com/astral-sh/ruff/issues/2152
synced_at: 2026-01-12T15:54:42Z
```

# Implement `pyupgrade` import replacements

---

_@stinodego_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

I noticed Ruff is missing this nice lint from pyupgrade:
https://github.com/asottile/pyupgrade#import-replacements

Basically, it replaces `from typing import Sequence` with the new `from collections.abc import Sequence` and similar.



---

_Comment by @charliermarsh on 2023-01-25 12:23_

There's a PR out for this (#2049) from @colin99d but I haven't had a chance to review it yet!

---

_Label `rule` added by @charliermarsh on 2023-01-25 12:23_

---

_Comment by @stinodego on 2023-01-25 12:25_

Oh great, thanks! I had missed that. Will keep an eye on that one.

Feel free to close this in favor of https://github.com/charliermarsh/ruff/issues/827 ; otherwise I'll close this myself once the PR is merged.

EDIT: Actually, I'll just close this in favor of #827. Thanks!

---

_Closed by @stinodego on 2023-01-25 12:27_

---
