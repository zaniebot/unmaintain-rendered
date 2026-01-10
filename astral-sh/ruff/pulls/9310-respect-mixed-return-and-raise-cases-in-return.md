```yaml
number: 9310
title: "Respect mixed `return` and `raise` cases in return-type analysis"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tweak
created_at: 2023-12-29T16:18:52Z
updated_at: 2023-12-29T16:56:02Z
url: https://github.com/astral-sh/ruff/pull/9310
synced_at: 2026-01-10T23:07:18Z
```

# Respect mixed `return` and `raise` cases in return-type analysis

---

_Pull request opened by @charliermarsh on 2023-12-29 16:18_

## Summary

Given:

```python
from somewhere import get_cfg

def lookup_cfg(cfg_description):
    cfg = get_cfg(cfg_description)
    if cfg is not None:
        return cfg
    raise AttributeError(f"No cfg found matching {cfg_description}")
```

We were analyzing the method from last-to-first statement. So we saw the `raise`, then assumed the method _always_ raised. In reality, though, it _might_ return. This PR improves the branch analysis to respect these mixed cases.

Closes https://github.com/astral-sh/ruff/issues/9269.
Closes https://github.com/astral-sh/ruff/issues/9304.

---

_Label `bug` added by @charliermarsh on 2023-12-29 16:18_

---

_Comment by @github-actions[bot] on 2023-12-29 16:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2023-12-29 16:46_

---

_Closed by @charliermarsh on 2023-12-29 16:46_

---

_Branch deleted on 2023-12-29 16:46_

---
