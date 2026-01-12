```yaml
number: 12886
title: "Improve docs for `missing-fstring-syntax` (`RUF027`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: ruf027-docs
created_at: 2024-08-14T09:19:42Z
updated_at: 2024-08-14T09:49:50Z
url: https://github.com/astral-sh/ruff/pull/12886
synced_at: 2026-01-12T15:55:42Z
```

# Improve docs for `missing-fstring-syntax` (`RUF027`)

---

_@AlexWaygood_

Some docs improvements I made while I was looking into whether we could stabilise this rule (which we decided in #12869 to postpone for now)

---

_Label `documentation` added by @AlexWaygood on 2024-08-14 09:19_

---

_Renamed from "Improve docs for RUF027" to "Improve docs for `missing-fstring-syntax` (`RUF027`)" by @AlexWaygood on 2024-08-14 09:23_

---

_Comment by @github-actions[bot] on 2024-08-14 09:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-14 09:47_

Nice. 

I guess anyone using fastapi will want to disable this rule or we'll have to add special fastapi handling to it

https://fastapi.tiangolo.com/tutorial/path-params/

---

_Comment by @AlexWaygood on 2024-08-14 09:49_

> Nice.
> 
> I guess anyone using fastapi will want to disable this rule or we'll have to add special fastapi handling to it
> 
> [fastapi.tiangolo.com/tutorial/path-params](https://fastapi.tiangolo.com/tutorial/path-params/)

Oh, interesting. I'm currently working on a separate PR to reduce some false positives in this rule (it also doesn't handle the fact that the stdlib `logging` module can accept template strings). I won't include fastAPI handling in that PR because the PR's nearly finished, but we could easily also special-case fastAPI as well.

---

_Merged by @AlexWaygood on 2024-08-14 09:49_

---

_Closed by @AlexWaygood on 2024-08-14 09:49_

---

_Branch deleted on 2024-08-14 09:49_

---
