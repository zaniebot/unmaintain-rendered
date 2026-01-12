```yaml
number: 10744
title: "[flake8_comprehensions] update docs for unnecessary-comprehension-any-all (C419)"
type: pull_request
state: merged
author: carljm
labels:
  - documentation
assignees: []
merged: true
base: main
head: fewercomps
created_at: 2024-04-02T22:57:08Z
updated_at: 2024-04-02T23:43:10Z
url: https://github.com/astral-sh/ruff/pull/10744
synced_at: 2026-01-12T15:55:33Z
```

# [flake8_comprehensions] update docs for unnecessary-comprehension-any-all (C419)

---

_@carljm_

Ref #3259; see in particular https://github.com/astral-sh/ruff/issues/3259#issuecomment-2033127339

## Summary

Improve the accuracy of the docs for this lint rule/fix.

## Test Plan

Generated the docs locally and visited the page for this rule:

![Screenshot 2024-04-02 at 4 56 40 PM](https://github.com/astral-sh/ruff/assets/61586/64f25cf6-edfe-4682-ac8e-7e21b834f5f2)



---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_any_all.rs`:36 on 2024-04-02 23:00_

```suggestion
/// The generator version is more memory-efficient.
```
? just for consistency

---

_@zanieb reviewed on 2024-04-02 23:00_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_any_all.rs`:34 on 2024-04-02 23:01_

Should the period be moved? Always awkward with a parenthetical at the end of a sentence.

```suggestion
/// necessarily greater than generator overhead).
```


---

_@zanieb reviewed on 2024-04-02 23:01_

---

_@zanieb approved on 2024-04-02 23:01_

---

_Label `documentation` added by @zanieb on 2024-04-02 23:02_

---

_Comment by @zanieb on 2024-04-02 23:03_

We use pull request labels for sorting changelog entries. Feel free to push branches to `astral-sh/ruff` if you want; we usually prefix with initials or name e.g. `zb/foo`.

---

_@carljm reviewed on 2024-04-02 23:06_

---

_Review comment by @carljm on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_any_all.rs`:34 on 2024-04-02 23:06_

Oh yeah, good spot; the parenthetical isn't its own sentence, so it should go outside in this case I think.

---

_Comment by @github-actions[bot] on 2024-04-02 23:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-04-02 23:42_

---

_Closed by @carljm on 2024-04-02 23:42_

---

_Branch deleted on 2024-04-02 23:43_

---
