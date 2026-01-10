```yaml
number: 12606
title: Add newlines before comments in E305
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/nl
created_at: 2024-08-01T02:31:17Z
updated_at: 2024-08-01T12:39:49Z
url: https://github.com/astral-sh/ruff/pull/12606
synced_at: 2026-01-10T21:47:02Z
```

# Add newlines before comments in E305

---

_Pull request opened by @charliermarsh on 2024-08-01 02:31_

## Summary

There's still a problem here. Given:

```python
class Class():
    pass

    # comment

    # another comment
a = 1
```

We only add one newline before `a = 1` on the first pass, because `max_precedling_blank_lines` is 1... We then add the second newline on the second pass, so it ends up in the right state, but the logic is clearly wonky.

Closes https://github.com/astral-sh/ruff/issues/11508.


---

_Label `bug` added by @charliermarsh on 2024-08-01 02:31_

---

_Comment by @github-actions[bot] on 2024-08-01 02:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-08-01 03:11_

---

_Closed by @charliermarsh on 2024-08-01 03:11_

---

_Branch deleted on 2024-08-01 03:11_

---

_Comment by @kaddkaka on 2024-08-01 05:41_

Are comments always tied to the next code line independent of indentation? Or does the indentation of a comment control where the empty lines are inserted? 

---

_Comment by @charliermarsh on 2024-08-01 12:39_

Should be the latter (controlled by indentation).

---
