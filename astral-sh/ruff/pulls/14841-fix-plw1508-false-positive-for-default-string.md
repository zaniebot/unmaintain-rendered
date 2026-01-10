```yaml
number: 14841
title: "Fix `PLW1508` false positive for default string created via a mult operation"
type: pull_request
state: merged
author: UnknownPlatypus
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-PLW1508-false_positive
created_at: 2024-12-08T17:57:06Z
updated_at: 2024-12-09T09:26:11Z
url: https://github.com/astral-sh/ruff/pull/14841
synced_at: 2026-01-10T20:42:27Z
```

# Fix `PLW1508` false positive for default string created via a mult operation

---

_Pull request opened by @UnknownPlatypus on 2024-12-08 17:57_

## Summary

Fix a false positive for default string created via a mult operation.
```python
MY_VAR = os.environ.get("MY_VAR", "0" * 32)
```

I find the `if matches!(op, Operator::Mult)` branch inside the `Operator::Mult` match quite inelegant but was not sure of the most idiomatic way to handle this.
## Test Plan

`cargo nextest run` and `cargo insta test`.




---

_Comment by @github-actions[bot] on 2024-12-08 18:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-12-08 18:21_

Thanks!

---

_Comment by @AlexWaygood on 2024-12-08 18:22_

> I find the `if matches!(op, Operator::Mult)` branch inside the `Operator::Mult` match quite inelegant but was not sure of the most idiomatic way to handle this.

Agreed -- I fixed it by splitting out the `Operator::Mult` handling into its own branch 👍 I think that makes more sense, even if it does introduce a tiny bit of repetition.

---

_Label `bug` added by @AlexWaygood on 2024-12-08 18:22_

---

_Merged by @AlexWaygood on 2024-12-08 18:25_

---

_Closed by @AlexWaygood on 2024-12-08 18:25_

---

_Comment by @UnknownPlatypus on 2024-12-09 09:26_

Wonderful thanks ! 

Btw, just wanted to say that the dev onboarding was really simple. The `Contributing.md` is doing a great job. 
Thanks for that.

---
