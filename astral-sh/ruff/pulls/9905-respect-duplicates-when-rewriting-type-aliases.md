```yaml
number: 9905
title: Respect duplicates when rewriting type aliases
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/type
created_at: 2024-02-09T03:11:58Z
updated_at: 2024-02-09T14:10:10Z
url: https://github.com/astral-sh/ruff/pull/9905
synced_at: 2026-01-10T22:57:09Z
```

# Respect duplicates when rewriting type aliases

---

_Pull request opened by @charliermarsh on 2024-02-09 03:11_

## Summary

If a generic appears multiple times on the right-hand side, we should only include it once on the left-hand side when rewriting.

Closes https://github.com/astral-sh/ruff/issues/9904.


---

_Review requested from @zanieb by @charliermarsh on 2024-02-09 03:12_

---

_Label `bug` added by @charliermarsh on 2024-02-09 03:12_

---

_Marked ready for review by @charliermarsh on 2024-02-09 03:12_

---

_Comment by @github-actions[bot] on 2024-02-09 03:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:50 on 2024-02-09 09:54_

It looks like we _are_ actually emitting a violation here, right? (And I think that's correct!)

```suggestion
# Make sure "T" appears only once
# in the type parameters for the modernized type alias
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:110 on 2024-02-09 09:56_

Since `Itertools` is already in scope, you could avoid the turbofish here:

```suggestion
        .collect_vec();
```

---

_@AlexWaygood approved on 2024-02-09 09:59_

LGTM

---

_@charliermarsh reviewed on 2024-02-09 13:56_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP040.py`:50 on 2024-02-09 13:56_

Yes, thank you!

---

_@charliermarsh reviewed on 2024-02-09 13:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep695_type_alias.rs`:110 on 2024-02-09 13:58_

As a habit, I try to avoid using `Itertools` where it's not providing much value since it (1) makes it a little easier if we remove the dep in the future and (2) in my opinion at least it's actually _easier_ to read the explicit version.

---

_Merged by @charliermarsh on 2024-02-09 14:02_

---

_Closed by @charliermarsh on 2024-02-09 14:02_

---

_Branch deleted on 2024-02-09 14:02_

---
