```yaml
number: 17810
title: "[red-knot] Add tests asserting that subclasses of `Any` are assignable to arbitrary protocol types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-any-subclass
created_at: 2025-05-03T12:39:19Z
updated_at: 2025-05-03T12:42:15Z
url: https://github.com/astral-sh/ruff/pull/17810
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Add tests asserting that subclasses of `Any` are assignable to arbitrary protocol types

---

_Pull request opened by @AlexWaygood on 2025-05-03 12:39_

## Summary

I thought we might have to add some new branches to `Type::is_assignable_to` here, similar to what we did for `Callable` types in #17717. But it turns out that it just falls out naturally from the logic we already have in place. A subclass of `Any` just returns a synthesized attribute of type `Any` for any member access where the member is not explicitly annotated on the subclass. And protocol subtyping/assignability works entirely through member access APIs on `Type`.

## Test Plan

this PR is only tests


---

_Label `red-knot` added by @AlexWaygood on 2025-05-03 12:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-03 12:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-03 12:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-03 12:39_

---

_Label `testing` added by @AlexWaygood on 2025-05-03 12:41_

---

_Merged by @AlexWaygood on 2025-05-03 12:41_

---

_Closed by @AlexWaygood on 2025-05-03 12:41_

---

_Branch deleted on 2025-05-03 12:41_

---

_Comment by @github-actions[bot] on 2025-05-03 12:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---
