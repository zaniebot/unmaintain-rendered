```yaml
number: 9651
title: "[`ruff`] Guard against use of `default_factory` as a keyword argument (`RUF026`)"
type: pull_request
state: merged
author: mikeleppane
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat(RUF026)/add-rule-for-defaultdict-with-defaulf-factory
created_at: 2024-01-26T15:03:52Z
updated_at: 2024-01-26T19:18:03Z
url: https://github.com/astral-sh/ruff/pull/9651
synced_at: 2026-01-10T22:57:09Z
```

# [`ruff`] Guard against use of `default_factory` as a keyword argument (`RUF026`)

---

_Pull request opened by @mikeleppane on 2024-01-26 15:03_

## Summary

Add a rule for defaultdict(default_factory=callable). Instead suggest using defaultdict(callable).

See: https://github.com/astral-sh/ruff/issues/9509

If a user tries to bind a "non-callable" to default_factory, the rule ignores it. Another option would be to warn that it's probably not what you want. Because Python allows the following:

```python 
from collections import defaultdict

defaultdict(default_factory=1)
```
this raises after you actually try to use it:

```python
dd = defaultdict(default_factory=1)
dd[1]
```
=> 
```bash
KeyError: 1
```

Instead using callable directly in the constructor it will raise (not being a callable):

```python 
from collections import defaultdict

defaultdict(1)
```
=> 
```bash
TypeError: first argument must be callable or None
```




## Test Plan

```bash
cargo test
```



---

_Comment by @github-actions[bot] on 2024-01-26 15:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @charliermarsh on 2024-01-26 15:51_

---

_Label `preview` added by @charliermarsh on 2024-01-26 15:51_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-01-26 15:51_

---

_@charliermarsh approved on 2024-01-26 19:02_

Thanks!

---

_Renamed from "[ruff]: Prefer using defaultdict(callable) instead of initializing with default_factory=callable (RUF026)" to "[`ruff`] Guard against use of `default_factory` as a keyword argument (`RUF026`)" by @charliermarsh on 2024-01-26 19:04_

---

_Merged by @charliermarsh on 2024-01-26 19:10_

---

_Closed by @charliermarsh on 2024-01-26 19:10_

---
