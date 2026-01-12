```yaml
number: 10669
title: Always place non-relative imports after relative imports
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2024-03-30T01:45:23Z
updated_at: 2024-03-30T02:13:55Z
url: https://github.com/astral-sh/ruff/pull/10669
synced_at: 2026-01-12T15:55:32Z
```

# Always place non-relative imports after relative imports

---

_@charliermarsh_

## Summary

When `relative-imports-order = "closest-to-furthest"` is set, we should _still_ put non-relative imports after relative imports. It's rare for them to be in the same section, but _possible_ if you use `known-local-folder`.

Closes https://github.com/astral-sh/ruff/issues/10655.

## Test Plan

New tests.

Also sorted this file:

```python
from ..models import ABC
from .models import Question
from .utils import create_question
from django_polls.apps.polls.models import Choice
```

With both:

- `isort view.py`
- `ruff check view.py --select I --fix`

And the following `pyproject.toml`:

```toml
[tool.ruff.lint.isort]
order-by-type = false
relative-imports-order = "closest-to-furthest"
known-local-folder = ["django_polls"]

[tool.isort]
profile = "black"
reverse_relative = true
known_local_folder = ["django_polls"]
```

I verified that Ruff and isort gave the same result, and that they _still_ gave the same result when removing the relevant setting:

```toml
[tool.ruff.lint.isort]
order-by-type = false
known-local-folder = ["django_polls"]

[tool.isort]
profile = "black"
known_local_folder = ["django_polls"]
```


---

_Label `bug` added by @charliermarsh on 2024-03-30 01:45_

---

_Label `isort` added by @charliermarsh on 2024-03-30 01:45_

---

_Comment by @github-actions[bot] on 2024-03-30 02:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-30 02:13_

---

_Closed by @charliermarsh on 2024-03-30 02:13_

---

_Branch deleted on 2024-03-30 02:13_

---
