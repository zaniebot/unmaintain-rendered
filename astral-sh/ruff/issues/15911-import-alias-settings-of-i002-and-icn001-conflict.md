```yaml
number: 15911
title: Import alias settings of I002 and ICN001 conflict even when they agree
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - configuration
assignees: []
created_at: 2025-02-03T14:18:23Z
updated_at: 2025-02-04T23:05:37Z
url: https://github.com/astral-sh/ruff/issues/15911
synced_at: 2026-01-12T15:54:55Z
```

# Import alias settings of I002 and ICN001 conflict even when they agree

---

_@dscorbett_

### Description

Ruff 0.9.4 reports a nonexistent conflict between the settings of [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) and [`unconventional-import-alias` (ICN001)](https://docs.astral.sh/ruff/rules/unconventional-import-alias/) when I002 requires an import with the alias ICN001 specifies. That makes it impossible to require an import that has a conventional alias.
```console
$ echo | ruff check --isolated --select I002,ICN001 --config 'lint.isort.required-imports = ["import numpy as np"]' - --fix
ruff failed
  Cause: Required import specified in `lint.isort.required-imports` (I002) conflicts with the required import alias specified in either `lint.flake8-import-conventions.aliases` or `lint.flake8-import-conventions.extend-aliases` (ICN001):
    - `numpy` -> `np`

Help: Remove the required import or alias from your configuration.
```

---

_Label `configuration` added by @AlexWaygood on 2025-02-03 15:55_

---

_Label `bug` added by @AlexWaygood on 2025-02-03 15:56_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-03 16:48_

---

_Closed by @dylwil3 on 2025-02-04 23:05_

---

_Closed by @dylwil3 on 2025-02-04 23:05_

---
