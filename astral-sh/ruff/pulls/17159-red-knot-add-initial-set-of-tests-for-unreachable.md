```yaml
number: 17159
title: "[red-knot] Add initial set of tests for unreachable code"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unreachable-code-tests
created_at: 2025-04-02T16:40:56Z
updated_at: 2025-04-02T17:43:46Z
url: https://github.com/astral-sh/ruff/pull/17159
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add initial set of tests for unreachable code

---

_@sharkdp_

## Summary

Add an initial set of tests that will eventually document our behavior around unreachable code. In the last section of this suite, I argue why we should never type check unreachable sections and never emit any diagnostics in these sections.


---

_Label `red-knot` added by @sharkdp on 2025-04-02 16:40_

---

_Review requested from @carljm by @sharkdp on 2025-04-02 16:40_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-02 16:40_

---

_Review requested from @dcreager by @sharkdp on 2025-04-02 16:40_

---

_Comment by @github-actions[bot] on 2025-04-02 16:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-04-02 16:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:242 on 2025-04-02 16:49_

I like that reasoning. I assume that only means we won't emit diagnostics. Would we still infer types for those sections? 

It will be interesting to see how we integrate this into the linter... but that's a problem for another day

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unreachable.md`:242 on 2025-04-02 17:04_

I think we _can_ still infer types for unreachable sections. Whether it's useful is less clear; you are likely to see a lot of `Never` types, since no binding from outside the unreachable code section can reach the unreachable code section.

---

_@carljm approved on 2025-04-02 17:05_

This all looks right to me.

---

_Comment by @carljm on 2025-04-02 17:13_

I suspect that a lot of "don't emit diagnostics in unreachable code" will fall out naturally from the prevalence of `Never` types, because `Never` (if we've correctly implemented it) is a very forgiving type: it is a subtype of everything, assignable to everything, has all attributes and methods (all of type `Never`), etc.

In fact it may be better to let it fall out like this than to implement a blanket policy of "no diagnostics in unreachable code". An argument could be made that in the cases where there is a binding _inside_ the unreachable code section, we should still emit diagnostics on invalid uses of that binding. For example, if `x = 3; y = x + "foo"` all occurs within unreachable code, there is no risk of false positive by still erroring on the bad binary operation. (The counter-argument would be the one mentioned in this PR: it can't cause issues at runtime. But I find that to be a less-convincing argument than the false-positive one. It can be useful to emit diagnostics for issues that cannot occur at runtime, if they _would_ occur at runtime given some other change. The `sys.version_info` cases are sort of an example of this.)

Unresolved-reference is an exception here; we definitely have to silence this in unreachable code, as a special case.

---

_Comment by @MichaReiser on 2025-04-02 17:14_

@carljm we may need special handling at least for `# type: ignore` or we'll get false-positives. 

---

_Comment by @carljm on 2025-04-02 17:14_

Yeah, "unused type-ignore" is another case, like unresolved-reference, that might need special handling.

---

_Comment by @AlexWaygood on 2025-04-02 17:16_

> Unresolved-reference is an exception here

I'm not at all sure we can predict which error codes are going to have false positives in unreachable code. This PR also has examples of `unresolved-attribute` and `unresolved-import` false-positive diagnostics occuring in unreachable code; there may be others, too.

---

_Comment by @carljm on 2025-04-02 17:20_

That may be true, but I don't think adding `unresolved-attribute` and `unresolved-import` to the list is strong evidence for that conclusion. Those are just variants of `unresolved-reference`; they're all issues about boundness, rather than type violations.

---

_Comment by @AlexWaygood on 2025-04-02 17:21_

Hmm, I suppose so. Are we _sure_ we won't add more error codes in the future that are similarly issues around boundness, though? It feels somewhat fragile to special-case specific error codes.

Anyway, we can iterate on it!

---

_Merged by @sharkdp on 2025-04-02 17:39_

---

_Closed by @sharkdp on 2025-04-02 17:39_

---

_Branch deleted on 2025-04-02 17:39_

---

_@AlexWaygood reviewed on 2025-04-02 17:43_

This is great, thanks!!

---
