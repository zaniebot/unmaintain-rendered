```yaml
number: 10720
title: Ignore annotated lambdas in class scopes
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/lambda-pos
created_at: 2024-04-01T18:04:48Z
updated_at: 2024-04-01T19:44:46Z
url: https://github.com/astral-sh/ruff/pull/10720
synced_at: 2026-01-12T15:55:33Z
```

# Ignore annotated lambdas in class scopes

---

_@charliermarsh_

## Summary

An annotated lambda assignment within a class scope is often intentional. For example, within a dataclass or Pydantic model, these are treated as fields rather than methods (and so can be passed values in constructors).

I originally wrote this to special-case dataclasses and Pydantic models... But was left feeling like we'd see more false positives here for little gain (an annotated lambda within a `class` is likely intentional?). Open to opinions, though.

Closes https://github.com/astral-sh/ruff/issues/10718.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-01 18:04_

---

_Label `rule` added by @charliermarsh on 2024-04-01 18:04_

---

_Comment by @github-actions[bot] on 2024-04-01 18:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-04-01 19:39_

What a weird edge case. This makes sense to me!

---

_Merged by @charliermarsh on 2024-04-01 19:44_

---

_Closed by @charliermarsh on 2024-04-01 19:44_

---

_Branch deleted on 2024-04-01 19:44_

---
