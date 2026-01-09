---
number: 10655
title: "isort and ruff behavior mismatch w.r.t. relative imports and `relative-imports-order` option."
type: issue
state: closed
author: Autopilot9369
labels:
  - bug
  - isort
assignees: []
created_at: 2024-03-29T10:17:25Z
updated_at: 2024-03-30T02:16:08Z
url: https://github.com/astral-sh/ruff/issues/10655
synced_at: 2026-01-07T13:12:15-06:00
---

# isort and ruff behavior mismatch w.r.t. relative imports and `relative-imports-order` option.

---

_Issue opened by @Autopilot9369 on 2024-03-29 10:17_

I have the following code:

```
# view.py

from .utils import create_question
from django_polls.apps.polls.models import Choice
from .models import Question
from ..models import ABC
```

and the following ruff config:
```
[tool.ruff.lint.isort]
case-sensitive = false
combine-as-imports = true
force-wrap-aliases = true
split-on-trailing-comma = true
default-section = "third-party"
detect-same-package = false
force-single-line = false
force-sort-within-sections = false
from-first = false
lines-between-types = 0
lines-after-imports = 2
extra-standard-library = ["typing_extensions"]
known-first-party = []
known-local-folder = ["django_polls"]
no-lines-before = ["future", "standard-library"]
order-by-type = false
relative-imports-order = "closest-to-furthest"
section-order = [
    "future",
    "standard-library",
    "third-party",
    "first-party",
    "local-folder",
]
```

now if I run `ruff check . --select I --fix`, the fixed code is as follows:

```
from django_polls.apps.polls.models import Choice
from .models import Question
from .utils import create_question
from ..models import ABC
```

but with the following isort config:
```
[tool.isort]
force_alphabetical_sort_within_sections = true
force_sort_within_sections = false
from_first = false
known_first_party = []
known_local_folder = ["django_polls"]
known_tests = ["tests"]
#line_length = 88
lines_after_imports = 2
lines_between_sections = 1
profile = "black"
reverse_relative = true
sections = [
    "FUTURE",
    "STDLIB",
    "THIRDPARTY",
    "FIRSTPARTY",
    "LOCALFOLDER",
]
src_paths = ["src", "tests"]
```

I get the following:

```
from .models import Question
from .utils import create_question
from ..models import ABC
from django_polls.apps.polls.models import Choice
```

I would prefer the output by isort, but I want to understand the behavior mismatch. I would assume that ruff's `relative-imports-order = "closest-to-furthest"` is equivalent to isort's `reverse_relative = true`. Please help me out. 
Why is the local folder (django_polls) imports treated as closest rather than relative `.` and `..` imports?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-30 00:50_

---

_Label `isort` added by @charliermarsh on 2024-03-30 01:00_

---

_Comment by @charliermarsh on 2024-03-30 01:17_

We probably just never tested because the "local folder" category typically only include relative imports. I would say our behavior is not quite correct, since the `relative-imports-order` should only affect relative imports, and not other imports in the "local folder" category.

---

_Label `bug` added by @charliermarsh on 2024-03-30 01:22_

---

_Referenced in [astral-sh/ruff#10669](../../astral-sh/ruff/pulls/10669.md) on 2024-03-30 01:45_

---

_Closed by @charliermarsh on 2024-03-30 02:13_

---

_Comment by @charliermarsh on 2024-03-30 02:16_

Fixed in the next release.

---

_Referenced in [inclusionAI/AReaL#494](../../inclusionAI/AReaL/pulls/494.md) on 2025-10-29 09:10_

---
