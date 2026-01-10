```yaml
number: 16299
title: "[`ruff`] Recognize `ClassVar` from superclasses correctly (`RUF045`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - bug
assignees: []
base: main
head: RUF045
created_at: 2025-02-21T10:30:53Z
updated_at: 2025-04-28T07:16:22Z
url: https://github.com/astral-sh/ruff/pull/16299
synced_at: 2026-01-10T19:03:00Z
```

# [`ruff`] Recognize `ClassVar` from superclasses correctly (`RUF045`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-21 10:30_

## Summary

Resolves #16297.

`RUF045` will now ignore an assignment if one of the following is true:

* At least one base class has an annotated assignment to the same target, where the annotation contains `ClassVar`, nested or otherwise.
* At least one base is not inspectable.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-21 10:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-02-21 14:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/implicit_classvar_in_dataclass.rs`:105 on 2025-02-21 14:56_

It seems undesired to compute this in the loop. Is there a reason for not computing it before the loop or after the loop and dropping all diagnostics in that case?

---

_@MichaReiser reviewed on 2025-02-21 15:06_

I left a question on the issue that I think we should answer first. I'm probably wrong but i'd like to understand the problem better first.

---

_@InSyncWithFoo reviewed on 2025-02-21 15:33_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/implicit_classvar_in_dataclass.rs`:105 on 2025-02-21 15:33_

Each iteration checks a different annotated assignment with a different `id`. It is indeed possible to refactor this, but if I were to do so I would prefer to wait until after #14688, which would add a few helpers for inspecting base classes.

---

_Comment by @MichaReiser on 2025-04-28 07:16_

Closing as catching this is the intention of the rule (see referenced issue). But thanks @InSyncWithFoo for kicking of the discussion.

---

_Closed by @MichaReiser on 2025-04-28 07:16_

---
