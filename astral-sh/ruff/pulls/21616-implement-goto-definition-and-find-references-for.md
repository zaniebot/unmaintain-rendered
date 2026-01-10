```yaml
number: 21616
title: Implement goto-definition and find-references for global/nonlocal statements
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/non-exprs
created_at: 2025-11-24T17:56:14Z
updated_at: 2025-11-25T13:56:59Z
url: https://github.com/astral-sh/ruff/pull/21616
synced_at: 2026-01-10T16:48:02Z
```

# Implement goto-definition and find-references for global/nonlocal statements

---

_Pull request opened by @Gankra on 2025-11-24 17:56_

## Summary

The implementation here is to just record the idents of these statements in `scopes_by_expression` (which already supported idents but only ones that happened to appear in expressions), so that `definitions_for_name` Just Works.

goto-type (and therefore hover) notably does not work on these statements because the typechecker does not record info for them. I am tempted to just introduce `type_for_name` which runs `definitions_for_name` to find other expressions and queries the inferred type... but that's a bit whack because it won't be the computed type at the right point in the code. It probably wouldn't be particularly expensive to just compute/record the type at those nodes, as if they were a load, because global/nonlocal is so scarce?

## Test Plan

Snapshot tests added/re-enabled.


---

_Comment by @astral-sh-bot[bot] on 2025-11-24 17:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-24 18:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-11-25 07:48_

Hmm, the changes in this PR seem to be primarily about string annotations and not nonlocal and global?

---

_Comment by @Gankra on 2025-11-25 13:17_

Yes it's based on top of my string attr PR which I have rebased without rebasing this one 😓 

---

_Marked ready for review by @Gankra on 2025-11-25 13:36_

---

_Review requested from @carljm by @Gankra on 2025-11-25 13:36_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-25 13:36_

---

_Review requested from @sharkdp by @Gankra on 2025-11-25 13:36_

---

_Review requested from @dcreager by @Gankra on 2025-11-25 13:36_

---

_Comment by @Gankra on 2025-11-25 13:36_

It is now rebased on main and ready to review.

---

_Label `server` added by @MichaReiser on 2025-11-25 13:41_

---

_Label `ty` added by @MichaReiser on 2025-11-25 13:41_

---

_@MichaReiser approved on 2025-11-25 13:44_

---

_Merged by @Gankra on 2025-11-25 13:56_

---

_Closed by @Gankra on 2025-11-25 13:56_

---

_Branch deleted on 2025-11-25 13:56_

---
