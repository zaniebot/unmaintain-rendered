```yaml
number: 10529
title: Respect Unicode characters in import sorting
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/isort
created_at: 2024-03-22T19:00:05Z
updated_at: 2024-03-22T19:16:50Z
url: https://github.com/astral-sh/ruff/pull/10529
synced_at: 2026-01-10T22:47:02Z
```

# Respect Unicode characters in import sorting

---

_Pull request opened by @charliermarsh on 2024-03-22 19:00_

## Summary

Ensures that we use the raw identifier as provided in the source code, rather than the normalized Unicode identifier.

This _does_ mean that we treat these as two separate identifiers, and _don't_ merge them, even though Python will treat them as the same symbol:

```python
import numpy as ℂℇℊℋℌℍℎℐℑℒℓℕℤΩℨKÅℬℭℯℰℱℹℴ
import numpy as CƐgHHHhIILlNZΩZKÅBCeEFio
```

I think that's fine, this is super rare anyway and would likely be confusing for users.

Closes https://github.com/astral-sh/ruff/issues/10528.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-03-22 19:00_

---

_Label `isort` added by @charliermarsh on 2024-03-22 19:00_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-03-22 19:00_

---

_@AlexWaygood approved on 2024-03-22 19:07_

This looks correct to me.

I worry that there might be similar issues in other rules that do autofixes -- it seems reasonable to rely on the `id` of a `Name` node being the same as in the original source, and then using it for autofixes. So, maybe eagerly normalizing unicode identifiers in the lexer is a bit of a bug magnet now ://

---

_@zanieb approved on 2024-03-22 19:10_

---

_Comment by @github-actions[bot] on 2024-03-22 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2024-03-22 19:16_

---

_Closed by @charliermarsh on 2024-03-22 19:16_

---

_Branch deleted on 2024-03-22 19:16_

---
