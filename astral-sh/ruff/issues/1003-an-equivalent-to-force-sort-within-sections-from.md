```yaml
number: 1003
title: "An equivalent to `force_sort_within_sections` from isort"
type: issue
state: closed
author: tmke8
labels:
  - isort
assignees: []
created_at: 2022-12-02T22:27:37Z
updated_at: 2023-01-09T06:06:57Z
url: https://github.com/astral-sh/ruff/issues/1003
synced_at: 2026-01-10T11:09:43Z
```

# An equivalent to `force_sort_within_sections` from isort

---

_Issue opened by @tmke8 on 2022-12-02 22:27_

I personally prefer to sort imports like this (which used to be the standard sorting for the Python extension on VS Code):
```python
import numpy as np
import torch
import torch.nn.functional as F
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor
import tqdm

from .my_module import my_func
```
as opposed to this sorting, which is how `isort` (and ruff) sorts by default:
```python
import numpy as np
import torch
import torch.nn.functional as F
import tqdm
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor

from .my_module import my_func
```
(Notice how `tqdm` is sandwiched there in the `torch` imports.)

With isort, I can achieve the desired sorting with the [force_sort_within_sections](https://pycqa.github.io/isort/docs/configuration/options.html#force-sort-within-sections) option. Would you be interested in supporting this as well?

---

_Renamed from "Equivalent to `force_sort_within_sections` from isort" to "An equivalent to `force_sort_within_sections` from isort" by @tmke8 on 2022-12-02 22:28_

---

_Comment by @charliermarsh on 2022-12-04 04:42_

Yeah we can probably support this (and I don't mind doing so), though it requires a bit of refactoring to the import model.

---

_Label `enhancement` added by @charliermarsh on 2022-12-04 04:42_

---

_Label `isort` added by @charliermarsh on 2022-12-04 04:42_

---

_Label `configuration` added by @charliermarsh on 2022-12-04 04:42_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:17_

---

_Label `configuration` removed by @charliermarsh on 2022-12-31 18:17_

---

_Comment by @charliermarsh on 2023-01-09 06:06_

Added in #1635.

---

_Closed by @charliermarsh on 2023-01-09 06:06_

---
