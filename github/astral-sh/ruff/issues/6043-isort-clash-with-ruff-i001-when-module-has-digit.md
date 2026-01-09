---
number: 6043
title: isort clash with ruff I001 when module has digit
type: issue
state: closed
author: mmarras
labels:
  - isort
  - needs-decision
assignees: []
created_at: 2023-07-24T21:10:28Z
updated_at: 2023-07-25T15:55:34Z
url: https://github.com/astral-sh/ruff/issues/6043
synced_at: 2026-01-07T13:12:15-06:00
---

# isort clash with ruff I001 when module has digit

---

_Issue opened by @mmarras on 2023-07-24 21:10_

isort sorts imports in a way that induces ruff to flag `I001`, aka ruff and isort want to sort the imports in different ways (I suspect when module has a number in name). 


after running isort
```python
from basic.views import datamore_login_required
from geln_main import config as config_main
from geln_olga2 import config
from geln_olga2.models import Data
from geln_olga.pytools import olga
from geln_olga.views import get_histogram_data, validate_parse_sp2files
from global_log_usage.views import create_hit
```

then 
```sh
geln_olga2/views.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

after running ruff auto-correct (which I only apply manually to see the difference)
```python
from basic.views import datamore_login_required
from geln_main import config as config_main
from geln_olga.pytools import olga
from geln_olga.views import get_histogram_data, validate_parse_sp2files
from geln_olga2 import config
from geln_olga2.models import Data
from global_log_usage.views import create_hit
```

Relevant settings are: 

.pre-commit-config.yaml
```yaml
- repo: https://github.com/pre-commit/mirrors-isort
    rev: v5.10.1
    hooks:
      - id: isort
        args: ["--profile", "black"]
- repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: "v0.0.280"
    hooks:
      - id: ruff
```


---

_Renamed from "isort clash with ruff I001" to "isort clash with ruff I001 when module has digit" by @mmarras on 2023-07-24 21:10_

---

_Comment by @charliermarsh on 2023-07-24 21:11_

Hah interesting

---

_Label `isort` added by @charliermarsh on 2023-07-24 21:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-24 21:11_

---

_Comment by @charliermarsh on 2023-07-24 21:13_

I'm trying to understand why isort would use that ordering. It's strange because (e.g.) `geln_olgaZ` (or any letter) would cause isort to use Ruff's import ordering. So it needs to be treating numerical suffixes as lexicographically lower.

---

_Comment by @zanieb on 2023-07-24 21:16_

Related discussion at https://github.com/PyCQA/isort/issues/1732 and implementation at https://github.com/PyCQA/isort/blob/615de51b1955d0b41e45de15081e20f03b3b708d/isort/sorting.py#L111-L130

---

_Comment by @charliermarsh on 2023-07-24 21:20_

Ahhh thank you, so helpful.

My gut reaction is that this looks like an unintended behavior in isort... They're trying to ensure that they respect _this_:

```python
from geln_olga2.models import Data
from geln_olga10.models import Data
```

But that's leading them to put numbers before non-numerical suffixes. Ruff seems to do the right thing -- it respects this:

```python
from geln_olga2.models import Data
from geln_olga10.models import Data
```

But also this:

```python
from geln_olga.models import Data
from geln_olga2.models import Data
```


---

_Comment by @charliermarsh on 2023-07-25 15:55_

I think I'm gonna close with the understanding that we do deviate from upstream when we feel that our behavior represents a bug fix.

---

_Closed by @charliermarsh on 2023-07-25 15:55_

---
