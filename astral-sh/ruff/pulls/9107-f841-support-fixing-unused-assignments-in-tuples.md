```yaml
number: 9107
title: "F841: support fixing unused assignments in tuples by renaming variables"
type: pull_request
state: merged
author: sciyoshi
labels:
  - fixes
assignees: []
merged: true
base: main
head: f841-rename-tuple
created_at: 2023-12-12T16:33:00Z
updated_at: 2023-12-12T18:56:48Z
url: https://github.com/astral-sh/ruff/pull/9107
synced_at: 2026-01-12T15:55:27Z
```

# F841: support fixing unused assignments in tuples by renaming variables

---

_@sciyoshi_

## Summary

A fairly common pattern which triggers F841 is unused variables from tuple assignments, e.g.:

    user, created = User.objects.get_or_create(...)
          ^ F841: Local variable `created` is assigned to but never used

This error is currently not auto-fixable.

This PR adds support for fixing the error automatically by renaming the unused variable to have a leading underscore (i.e. `_created`) **iff** the `dummy-variable-rgx` setting would match it.

I considered using `renamers::Renamer` here, but because by the nature of the error there should be no references to it, that seemed like overkill. Also note that the fix might break by shadowing the new name if it is already used elsewhere in the scope. I left it as is because

1. the renamed variable matches the "unused" regex, so it should hopefully not already be used,
2. the fix is marked as unsafe so it should be reviewed manually anyways, and
3. I'm not actually sure how to check the scope for the new variable name 😅 

---

_Comment by @github-actions[bot] on 2023-12-12 17:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-12-12 18:22_

Nice, this makes sense to me. Appreciate the clear rationale too!

---

_Label `autofix` added by @charliermarsh on 2023-12-12 18:23_

---

_Merged by @charliermarsh on 2023-12-12 18:23_

---

_Closed by @charliermarsh on 2023-12-12 18:23_

---

_Branch deleted on 2023-12-12 18:56_

---
