```yaml
number: 4195
title: "[feature] remove unused imports"
type: issue
state: closed
author: upstartjohnvandivier
labels:
  - question
assignees: []
created_at: 2023-05-02T17:47:38Z
updated_at: 2023-05-04T15:35:41Z
url: https://github.com/astral-sh/ruff/issues/4195
synced_at: 2026-01-10T11:09:47Z
```

# [feature] remove unused imports

---

_Issue opened by @upstartjohnvandivier on 2023-05-02 17:47_

currently when i run autofix on a large codebase in py 3.7, `from __future__ import annotations` gets added and Dict, List, Tuple, Union...many others are no longer used but their imports remain in many files

at a minimum, cleaning up these specific changes would be helpful, and a general remove unused import change would be even better

related iSort issue:
https://github.com/PyCQA/isort/issues/1105

---

_Comment by @charliermarsh on 2023-05-03 06:02_

We do provide a rule for unused imports, and it can remove unused imports automatically too (F401). Does that work for your use-case?

---

_Label `question` added by @charliermarsh on 2023-05-03 06:02_

---

_Comment by @charliermarsh on 2023-05-04 15:35_

Closing but let me know if I've misunderstood.

---

_Closed by @charliermarsh on 2023-05-04 15:35_

---
