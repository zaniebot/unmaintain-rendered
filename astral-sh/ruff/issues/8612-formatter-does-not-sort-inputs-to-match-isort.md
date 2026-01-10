```yaml
number: 8612
title: "Formatter does not sort inputs to match isort profile [\"I\"]"
type: issue
state: closed
author: mwip
labels:
  - question
  - isort
assignees: []
created_at: 2023-11-11T09:18:33Z
updated_at: 2023-11-11T13:57:35Z
url: https://github.com/astral-sh/ruff/issues/8612
synced_at: 2026-01-10T11:09:50Z
```

# Formatter does not sort inputs to match isort profile ["I"]

---

_Issue opened by @mwip on 2023-11-11 09:18_

This could be by design, but I did not find a specific answer to the following question in the issues here. So I thought I'd ask.

Is there any option to force import sorting in `ruff format` in below's example? I am aware that `ruff test.py --fix` would solve this, but to me, this seems like an unnecessary second step. (Using a `lsp` integration this could mean a manual intervention when it could also be used in an on-save-hook, for example.) Am I maybe missing something in the [conflicting lint rules](https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules)?

Given the following MWE:

```python
# test.py
import os
import sys
from pathlib import Path
import numpy as np
from local_module import something
```
```toml
# pyproject.toml
[tool.ruff]
select = [
    "I",
]
```
we can see `ruff` will check for sorted inputs:
```shell
$ ruff test.py
# test.py:1:1: I001 [*] Import block is un-sorted or un-formatted
# Found 1 error.
# [*] 1 fixable with the `--fix` option.
```
Yet,  `ruff format` will not sort imports `isort`-style:
```shell
$ ruff format test.py
# 1 file left unchanged
```
This would be the expected outcome:
```python
import os
import sys
from pathlib import Path

import numpy as np

from local_module import something
```

---

_Comment by @charliermarsh on 2023-11-11 12:34_

Good question -- we have an open issue to document this better (#8367).

Right now, import sorting is part of the linter (`ruff check` and `ruff check --fix`) rather than the formatter `ruff format`). At present, the formatter doesn't do any import _sorting_ -- just formatting. So it won't reorder and collapse imports. This is both for historical reasons and because unlike the rest of the formatter, it requires changing the AST and so (possibly) the behavior of your code.

More practically, what this means is that import sorting is performed via `ruff check` and `ruff check --fix` rather than `ruff format`. With the above configuration, `ruff check --fix` should give you isort-like import sorting, while `ruff format` will handle general code formatting.

---

_Closed by @charliermarsh on 2023-11-11 12:34_

---

_Label `question` added by @charliermarsh on 2023-11-11 12:34_

---

_Label `isort` added by @charliermarsh on 2023-11-11 12:34_

---

_Comment by @charliermarsh on 2023-11-11 12:34_

(I'm happy to answer any follow-up questions of course, just ask :))

---

_Comment by @mwip on 2023-11-11 13:57_

Thank you for the prompt and helpful reply. This totally makes sense. Excuse me missing the issue and thanks for linking it.

I will just set up my on save hooks to run `ruff check --select "I" --fix` for the file in addition to a `ruff format`. Works fine for me. (I have to make use of Emacs' customizability in some way :wink: ). Maybe this help someone else finding this in the future.

---
