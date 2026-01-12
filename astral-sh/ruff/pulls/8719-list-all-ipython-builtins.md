```yaml
number: 8719
title: List all ipython builtins
type: pull_request
state: merged
author: konstin
labels:
  - linter
assignees: []
merged: true
base: main
head: ipython-builtins-script
created_at: 2023-11-16T12:32:50Z
updated_at: 2023-11-16T18:06:26Z
url: https://github.com/astral-sh/ruff/pull/8719
synced_at: 2026-01-10T23:40:55Z
```

# List all ipython builtins

---

_Pull request opened by @konstin on 2023-11-16 12:32_

I checked for ipython-specific builtins on python 3.11 using
```python
import json
from subprocess import check_output

builtins_python = json.loads(check_output(["python3", "-c" "import json; print(json.dumps(dir(__builtins__)))"]))
builtins_ipython = json.loads(check_output(["ipython3", "-c" "import json; print(json.dumps(dir(__builtins__)))"]))
print(sorted(set(builtins_ipython) - set(builtins_python)))
```
and updated the relevant constant and match. The list changes from

`display`

to

`__IPYTHON__`, `display`, `get_ipython`.

Followup to #8707

---

_Label `linter` added by @konstin on 2023-11-16 12:32_

---

_Comment by @github-actions[bot] on 2023-11-16 12:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-16 17:33_

Thank you!

---

_Merged by @konstin on 2023-11-16 18:06_

---

_Closed by @konstin on 2023-11-16 18:06_

---

_Branch deleted on 2023-11-16 18:06_

---
