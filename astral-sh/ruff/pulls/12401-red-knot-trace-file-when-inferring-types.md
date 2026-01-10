```yaml
number: 12401
title: "[red-knot] trace file when inferring types"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/better-tracing
created_at: 2024-07-19T01:26:04Z
updated_at: 2024-07-19T14:13:54Z
url: https://github.com/astral-sh/ruff/pull/12401
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] trace file when inferring types

---

_Pull request opened by @carljm on 2024-07-19 01:26_

When poring over traces, the ones that just include a definition or symbol or expression ID aren't very useful, because you don't know which file it comes from. This adds that information to the trace.

I guess the downside here is that if calling `.file(db)` on a scope/definition/expression would execute other traced code, it would be marked as outside the span? I don't think that's a concern, because I don't think a simple field access on a tracked struct should ever execute our code. If I'm wrong and this is a problem, it seems like the tracing crate has this feature where you can record a field as `tracing::field::Empty` and then fill in its value later with `span.record(...)`, but when I tried this it wasn't working for me, not sure why.

I think there's a lot more we can do to make our tracing output more useful for debugging (e.g. record an event whenever a definition/symbol/expression/use id is created with the details of that definition/symbol/expression/use), this is just dipping my toes in the water.


---

_Label `red-knot` added by @carljm on 2024-07-19 01:26_

---

_Review requested from @MichaReiser by @carljm on 2024-07-19 01:26_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-19 01:26_

---

_Comment by @github-actions[bot] on 2024-07-19 01:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-07-19 05:50_

---

_Comment by @MichaReiser on 2024-07-19 05:51_

> I think there's a lot more we can do to make our tracing output more useful for debugging (e.g. record an event whenever a definition/symbol/expression/use id is created with the details of that definition/symbol/expression/use), this is just dipping my toes in the water.

Possibly, but I think it would make the trace output so verbose that it is no longer useful when working on other parts of the code.

---

_@AlexWaygood approved on 2024-07-19 12:44_

---

_Merged by @carljm on 2024-07-19 14:13_

---

_Closed by @carljm on 2024-07-19 14:13_

---

_Branch deleted on 2024-07-19 14:13_

---
