```yaml
number: 12604
title: "Don't attach comments with mismatched indents"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/start
created_at: 2024-08-01T01:45:47Z
updated_at: 2024-08-01T02:09:07Z
url: https://github.com/astral-sh/ruff/pull/12604
synced_at: 2026-01-10T21:47:02Z
```

# Don't attach comments with mismatched indents

---

_Pull request opened by @charliermarsh on 2024-08-01 01:45_

## Summary

Given:

```python
def test_update():
    pass
    # comment
def test_clientmodel():
    pass
```

We don't want `# comment` to be attached to `def test_clientmodel()`.

Closes https://github.com/astral-sh/ruff/issues/12589.


---

_Label `bug` added by @charliermarsh on 2024-08-01 01:45_

---

_Comment by @github-actions[bot] on 2024-08-01 01:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-08-01 02:09_

---

_Closed by @charliermarsh on 2024-08-01 02:09_

---

_Branch deleted on 2024-08-01 02:09_

---
