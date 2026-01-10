```yaml
number: 14843
title: "[flake8-commas]: Make the recommended replacement actually a tuple"
type: pull_request
state: merged
author: mitya57
labels:
  - documentation
assignees: []
merged: true
base: main
head: flake8-commas
created_at: 2024-12-08T19:39:15Z
updated_at: 2024-12-09T00:51:59Z
url: https://github.com/astral-sh/ruff/pull/14843
synced_at: 2026-01-10T20:42:27Z
```

# [flake8-commas]: Make the recommended replacement actually a tuple

---

_Pull request opened by @mitya57 on 2024-12-08 19:39_

## Summary

Minor change for the documentation of COM818 rule. This was a block called “In the event that a tuple is intended”, but the suggested change did not produce a tuple.

## Test Plan

```python
>>> import json
>>> (json.dumps({"bar": 1}),)  # this is a tuple
('{"bar": 1}',)
>>> (json.dumps({"bar": 1}))  # not a tuple
'{"bar": 1}'
```

---

_Comment by @github-actions[bot] on 2024-12-08 19:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-09 00:51_

Thank you!

---

_Label `documentation` added by @AlexWaygood on 2024-12-09 00:51_

---

_Merged by @AlexWaygood on 2024-12-09 00:51_

---

_Closed by @AlexWaygood on 2024-12-09 00:51_

---
