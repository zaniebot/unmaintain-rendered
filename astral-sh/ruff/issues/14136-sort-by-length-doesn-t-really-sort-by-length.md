---
number: 14136
title: "Sort By Length Doesn't really sort by length"
type: issue
state: closed
author: scarere
labels:
  - isort
assignees: []
created_at: 2024-11-06T15:59:02Z
updated_at: 2024-11-08T03:40:37Z
url: https://github.com/astral-sh/ruff/issues/14136
synced_at: 2026-01-10T01:22:54Z
---

# Sort By Length Doesn't really sort by length

---

_Issue opened by @scarere on 2024-11-06 15:59_

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

Not sure if this issue should be raised with isort or ruff, but since ruff is what I'm using raising it here. With the following ruff.toml file
```toml
fix = true
extend-select = ["I"]
extend-safe-fixes = ["I"]

# Add NOTE to default task tags
task-tags = ["TODO", "FIXME", "XXX", "NOTE"]

[lint.pycodestyle]
# Ignore overlong comments if they start with a task tag
ignore-overlong-task-comments = true

[lint.isort]
length-sort = true
```

ruff sorts my imports as follows
```python
import os
import contextlib
from typing import List, Tuple
from logging import INFO
from os.path import join, isdir
from pathlib import Path

import yaml
import torch
import nnunetv2
from tqdm import tqdm
from nnunetv2.paths import nnUNet_raw
from flwr.common.logger import log
from nnunetv2.ensembling.ensemble import ensemble_folders
from nnunetv2.utilities.find_class_by_name import recursive_find_python_class
from nnunetv2.inference.predict_from_raw_data import nnUNetPredictor
from nnunetv2.utilities.plans_handling.plans_handler import PlansManager
from nnunetv2.utilities.label_handling.label_handling import (
    determine_num_input_channels,
)
```

It is sorting the `import x` imports seperately from the `from x import y` imports. Additionally, within the `from` imports, it is sorting based only on the line length of the module, not the entire line length. I tried `setting force-sort-within-sections=true` but this seems to override the length-sort and sort alphabetically as seen below.

```python
import os
from typing import List, Tuple
from logging import INFO
from os.path import join, isdir
from pathlib import Path
import contextlib

from tqdm import tqdm
import yaml
import torch
import nnunetv2
from nnunetv2.paths import nnUNet_raw
from flwr.common.logger import log
from nnunetv2.ensembling.ensemble import ensemble_folders
from nnunetv2.utilities.find_class_by_name import recursive_find_python_class
from nnunetv2.inference.predict_from_raw_data import nnUNetPredictor
from nnunetv2.utilities.plans_handling.plans_handler import PlansManager
from nnunetv2.utilities.label_handling.label_handling import (
    determine_num_input_channels,
)
```

My expected output for `length-sort=true` and `force-sort-within-sections=true` would be

```python
import os
import contextlib
from logging import INFO
from pathlib import Path
from typing import List, Tuple
from os.path import join, isdir

import yaml
import torch
import nnunetv2
from tqdm import tqdm
from flwr.common.logger import log
from nnunetv2.paths import nnUNet_raw
from nnunetv2.ensembling.ensemble import ensemble_folders
from nnunetv2.utilities.label_handling.label_handling import (
    determine_num_input_channels,
)
from nnunetv2.inference.predict_from_raw_data import nnUNetPredictor
from nnunetv2.utilities.plans_handling.plans_handler import PlansManager
from nnunetv2.utilities.find_class_by_name import recursive_find_python_class
```

To take this one step further, my actual ideal sort order would support some 'overriding rules' such as keeping imports from the same package together, and having imports that span multiple lines at the bottom. I don't know how complicated this would be to implement but it would be great if we could have sorting rules and order them based on priority. Something such as:

```python
import os
import contextlib
from logging import INFO
from pathlib import Path
from typing import List, Tuple
from os.path import join, isdir

import yaml
import torch
import nnunetv2
from tqdm import tqdm
from flwr.common.logger import log
from nnunetv2.paths import nnUNet_raw
from nnunetv2.ensembling.ensemble import ensemble_folders
from nnunetv2.inference.predict_from_raw_data import nnUNetPredictor
from nnunetv2.utilities.plans_handling.plans_handler import PlansManager
from nnunetv2.utilities.find_class_by_name import recursive_find_python_class
from nnunetv2.utilities.label_handling.label_handling import (
    determine_num_input_channels,
)
```

I could create a custom section for all the nnunet imports to keep them grouped together, but the other problems remain. Is there a way for me to get the expected or desired behaviour, if not would this be a hard feature to add?

---

_Label `isort` added by @AlexWaygood on 2024-11-06 17:26_

---

_Comment by @charliermarsh on 2024-11-06 17:48_

This seems to match isort's behavior, so I'd prefer not to change it.

---

_Closed by @charliermarsh on 2024-11-08 03:40_

---

_Referenced in [PyCQA/isort#2304](../../PyCQA/isort/issues/2304.md) on 2024-11-20 15:14_

---

_Referenced in [astral-sh/ruff#9870](../../astral-sh/ruff/issues/9870.md) on 2025-12-05 19:23_

---
