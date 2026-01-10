```yaml
number: 11972
title: "[red-knot] Migrate CLI to Salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: salsa-10-red-knot-cli
created_at: 2024-06-21T16:15:04Z
updated_at: 2024-07-04T07:38:19Z
url: https://github.com/astral-sh/ruff/pull/11972
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Migrate CLI to Salsa

---

_Pull request opened by @MichaReiser on 2024-06-21 16:15_

## Summary

This PR mainly deletes code that is now in one of the downstream crates or has been replaced by Salsa. 

## What's missing

The program currently panics when Salsa cancels a pending query. I [reached out in the Salsa zulip](https://salsa.zulipchat.com/#narrow/stream/145099-general/topic/How.20to.20use.20.60Cancelled.3A.3Acatch.60) to learn out how to use `Cancelled::catch` correctly.

## What I removed

I removed the code for scheduling dependencies. Mainly because I'm not sure if it still makes sense to do so:

* The main benefit is that we can load and parse dependencies concurrently. We may still want to do that
* I don't think it makes sense with the new definition-level inference to eagerly schedule type checking of third-party dependencies. We otherwise end up inferring more types than we have to (let's say you only import a single symbol from a dependency, it then doesn't make sense to type the entire file and all it's dependencies, although we might have to)
* I think we want to check all workspace files concurrently, but the current CLI only supports a single file. 

The main downside of not scheduling dependencies anymore is if the user only passes exactly one input file. Ruff might then infer the types of all third-party dependencies and the stdlib on a single thread. This doesn't sound ideal.

We should revisit the scheduling when working on workspace support.

## Test Plan

```
[crates/red_knot/src/main.rs:173:21] diagnostics = [
    "Unresolved import 'Base1'",
    "Method A.foo is decorated with `typing.override` but does not override any base class method",
]
```


---

_Label `red-knot` added by @MichaReiser on 2024-06-21 16:15_

---

_Comment by @github-actions[bot] on 2024-07-02 14:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2024-07-03 13:37_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:23 on 2024-07-03 13:37_

FIXME

---

_Marked ready for review by @MichaReiser on 2024-07-03 13:43_

---

_Review requested from @carljm by @MichaReiser on 2024-07-03 13:43_

---

_Review comment by @carljm on `crates/red_knot/src/lib.rs`:44 on 2024-07-03 22:06_

Pre-existing issue, but this comment looks pretty unrelated to the code

---

_@carljm approved on 2024-07-03 22:18_

---

_Comment by @carljm on 2024-07-03 22:19_

Some clippy complaints to address before landing.

---

_Merged by @MichaReiser on 2024-07-04 07:23_

---

_Closed by @MichaReiser on 2024-07-04 07:23_

---

_Branch deleted on 2024-07-04 07:23_

---
