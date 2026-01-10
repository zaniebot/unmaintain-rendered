```yaml
number: 2595
title: Would be nice to have more support for isort options.
type: issue
state: closed
author: mikelane
labels:
  - isort
assignees: []
created_at: 2023-02-05T23:07:12Z
updated_at: 2023-05-10T05:17:10Z
url: https://github.com/astral-sh/ruff/issues/2595
synced_at: 2026-01-10T11:09:45Z
```

# Would be nice to have more support for isort options.

---

_Issue opened by @mikelane on 2023-02-05 23:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
In the imports, it's nice to have them all on separate lines. Using isort directly, I tend to use the following settings:

```toml
[tool.isort]
profile = "black"
multi_line_output = 3  # vertial hanging indent
force_grid_wrap = 2  # Force multiline when there are 2+ imports from the same module
```

This results in imports that look like this:

```python
from functools import cache
from itertools import (
    combinations,
    permutations,
)

from pydantic import (
    BaseModel,
    EmailStr,
)
```

And then if I add an import from itertools, I can see the changed import quickly in the PR because that change will have its own line. 

As far as I can tell, this setting isn't included in ruff. I'm loving what I'm seeing with ruff, so I'd love to have the option of setting the `multi_line_output` and `force_grid_wrap` options in ruff.

---

_Comment by @charliermarsh on 2023-02-05 23:50_

Do you mind filing individual issues for these? I can't promise that we'll support them, but it's a little easier to track that way :)

---

_Closed by @charliermarsh on 2023-02-05 23:50_

---

_Label `isort` added by @charliermarsh on 2023-02-05 23:50_

---

_Comment by @dhirschfeld on 2023-05-10 05:16_

***xref:***
  * https://github.com/charliermarsh/ruff/issues/2601
  * https://github.com/charliermarsh/ruff/issues/2600

---
