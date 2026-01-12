```yaml
number: 8853
title: Add advice for fixing RUF008 when mutability is not desired
type: pull_request
state: merged
author: samueljsb
labels:
  - documentation
assignees: []
merged: true
base: main
head: mutable-class-default-immutable-advice
created_at: 2023-11-27T14:30:20Z
updated_at: 2023-11-27T18:02:41Z
url: https://github.com/astral-sh/ruff/pull/8853
synced_at: 2026-01-12T15:55:27Z
```

# Add advice for fixing RUF008 when mutability is not desired

---

_@samueljsb_

## Summary

A common mistake in Python code is creating global mutable state by assigning class attributes that are mutable (e.g. a list or dictionary). `RUF012` helpfully catches this mistake and can be used to prevent inadvertent global state form being created.

The description of that rule helpfully provides advice for adjusting code to stop beigng reported as an error *if the attribute should be mutable*, but does not say how the error can be avoided when mutability is not the goal.

This change adds that advice, so that both possibilities are accounted for.

## Test Plan

Docs built and inspected visually:

![Screenshot 2023-11-27 at 14 30 02](https://github.com/astral-sh/ruff/assets/7549858/5bcf2f06-38ed-43e5-adab-bf8fc5992c3d)


---

_Comment by @github-actions[bot] on 2023-11-27 14:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @zanieb on 2023-11-27 15:27_

---

_@zanieb approved on 2023-11-27 15:27_

Thanks!

---

_Merged by @zanieb on 2023-11-27 15:27_

---

_Closed by @zanieb on 2023-11-27 15:27_

---

_Branch deleted on 2023-11-27 18:02_

---
