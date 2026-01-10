```yaml
number: 11832
title: Suppress isort for single import line
type: issue
state: closed
author: bloodgain
labels:
  - question
assignees: []
created_at: 2024-06-10T21:41:02Z
updated_at: 2024-06-11T23:33:07Z
url: https://github.com/astral-sh/ruff/issues/11832
synced_at: 2026-01-10T11:09:53Z
```

# Suppress isort for single import line

---

_Issue opened by @bloodgain on 2024-06-10 21:41_

Is is possible to tell ruff to ignore a single line for isort plugin sorting?

Currently, it appears that ruff can't process `# noqa: I001` for single lines. The only way I have found to disable import sorting at all in a file is to place `# ruff: noqa: I001` before the first import, which ignores `unsorted-imports` for the entire file.

Example:
```python
# ruff: noqa: I001 <-- ignores import sorting for whole file, and must start with "ruff:", which isn't ideal
import os
import shutil
from time import time

import ray
# ... more RLlib import stuff

# local imports
from customModels import BaseCustomPPO
# ... etc.

# must be imported out of order due to GLIBCXX error with matplotlib
# noqa: I001 <-- not respected here
import pandas as pd  # noqa: I001 <-- not respected here, either
```
 
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


---

_Comment by @charliermarsh on 2024-06-10 21:47_

You need to put `# noqa: I001` on the first line of the import _block_ (i.e., the first line of the violation).

---

_Comment by @bloodgain on 2024-06-10 23:27_

Unfortunately, it appears that it always triggers on the first place it thinks `import` is out-of-order, even with the intentional out-of-order import in a separate block with a `# noqa` guard. Using `--fix` results in it moving the `noqa` guard comment with the import.

Full import example (I let ruff sort the other imports to narrow the problem.):
**BEFORE**
```python
import os
import shutil
from time import time

import ray

ray.init(local_mode=True)
import wsf_python as alt_sim
from ray.rllib.algorithms.algorithm import Algorithm
from ray.rllib.models import ModelCatalog

from customModels import BaseCustomPPO
from models import getModel
from utilities import CSVLogging, orderData, registerRLLib

# noqa: I001
import pandas as pd  #must be imported out of order due to GLIBCXX error with matplotlib
```

**CHECK**
```bash
>$ ruff check --select=I001 ./reinforcementLearning/evaluate.py
reinforcementLearning\evaluate.py:8:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

**AFTER `--fix`**
```python
import os
import shutil
from time import time

import ray

ray.init(local_mode=True)
# noqa: I001
import pandas as pd  #must be imported out of order due to GLIBCXX error with matplotlib
import wsf_python as alt_sim
from ray.rllib.algorithms.algorithm import Algorithm
from ray.rllib.models import ModelCatalog

from customModels import BaseCustomPPO
from models import getModel
from utilities import CSVLogging, orderData, registerRLLib
```

Changing the guard comment to `# ruff: noqa: I001` ignores the rule for the whole file, even though it's below the other imports.

NOTE: I am not assuming this is a bug, just an artifact of how isort/ruff.lint.isort operates. I couldn't find any examples for ignoring a single out-of-order import, so I thought this was the best place to find out if I missed the correct way to do it or it wasn't possible.

---

_Comment by @dhruvmanila on 2024-06-11 06:13_

Assuming that you _just_ want to ignore the `pandas` import, can you try it with `isort: skip`?

```py
import os
import shutil
from time import time

import ray

ray.init(local_mode=True)
import wsf_python as alt_sim
from ray.rllib.algorithms.algorithm import Algorithm
from ray.rllib.models import ModelCatalog

from customModels import BaseCustomPPO
from models import getModel
from utilities import CSVLogging, orderData, registerRLLib

import pandas as pd  # isort: skip  # must be imported out of order due to GLIBCXX error with matplotlib	
```

This works for me locally.

The `noqa: I001` comment should be at the start of the import block which would be:
```py
import os
import shutil
from time import time

import ray

ray.init(local_mode=True)
import wsf_python as alt_sim  # noqa: I001
from ray.rllib.algorithms.algorithm import Algorithm
from ray.rllib.models import ModelCatalog

from customModels import BaseCustomPPO
from models import getModel
from utilities import CSVLogging, orderData, registerRLLib

import pandas as pd  # must be imported out of order due to GLIBCXX error with matplotlib	
```

But, this would ignore the entire block which includes all the imports after `ray.init` call. The `noqa` comment cannot be on its own line either.

Reference: https://docs.astral.sh/ruff/linter/#error-suppression

---

_Label `question` added by @dhruvmanila on 2024-06-11 06:13_

---

_Comment by @bloodgain on 2024-06-11 18:43_

@dhruvmanila 
Ah, so! That works!

Exactly what we needed. Thank you very much! 😊🙏

---

_Comment by @charliermarsh on 2024-06-11 23:33_

Thank you @dhruvmanila!

---

_Closed by @charliermarsh on 2024-06-11 23:33_

---
