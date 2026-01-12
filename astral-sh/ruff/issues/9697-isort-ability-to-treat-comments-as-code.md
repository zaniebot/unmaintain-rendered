```yaml
number: 9697
title: isort ability to treat comments as code
type: issue
state: closed
author: yasirroni
labels:
  - question
  - isort
assignees: []
created_at: 2024-01-30T05:43:59Z
updated_at: 2024-09-02T15:18:07Z
url: https://github.com/astral-sh/ruff/issues/9697
synced_at: 2026-01-12T15:54:49Z
```

# isort ability to treat comments as code

---

_@yasirroni_

isort support ability to treat comments as code https://github.com/pycqa/isort/issues/1357. It is very useful in `.py` files generated from `.ipynb` that each section of import separated by comment `# %%`


Thus, 
```python
import os

import cvxpy as cp
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# %%  >>> This should make all import below this treated as separate isort task
import sys
import traceback
import warnings
```
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `question` added by @MichaReiser on 2024-01-30 07:15_

---

_Label `isort` added by @MichaReiser on 2024-01-30 07:15_

---

_Comment by @dhruvmanila on 2024-01-30 11:04_

Thanks for the feature request!

I agree that having this config would be useful for this specific use-case but I was wondering what's the motive behind running Ruff on the generated `.py` file instead of `.ipynb` directly.

We do something like this for Jupyter notebooks where we need to treat each cell boundary as an import boundary. This can then be used here by treating comments as another import boundary.

---

_Comment by @yasirroni on 2024-01-30 17:02_

That would make sense. I just need to exclude all generated py file. Thanks.

---

_Comment by @charliermarsh on 2024-02-08 03:33_

I think I'd prefer not to support this until we see a compelling reason to do so, since we support import sorting within notebooks natively. Gonna close for now.

---

_Closed by @charliermarsh on 2024-02-08 03:33_

---

_Comment by @mwoitek on 2024-09-02 15:18_

I've found this issue because I'd like to have this feature. So I want to give you a reason to add it. I don't like to use Jupyter notebooks for coding. I avoid them with the help of a tool named Jupytext. It allows me to create notebooks from Python scripts. To do so, I add to my scripts special comments that serve as cell delimiters. Then it's important that, when I format my code, these comments remain in place. That's precisely the problem that isort's `treat_comments_as_code` solves. Please consider adding this option.

---
