```yaml
number: 21368
title: "[ty] supress inlay hints for `+1` and `-1`"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/inlay-unary
created_at: 2025-11-10T20:13:46Z
updated_at: 2025-11-13T19:16:23Z
url: https://github.com/astral-sh/ruff/pull/21368
synced_at: 2026-01-12T15:57:21Z
```

# [ty] supress inlay hints for `+1` and `-1`

---

_@Gankra_

It's everyone's favourite language corner case!

Also having kicked the tires on it, I'm pretty happy to call this (in conjunction with #21367):

Fixes https://github.com/astral-sh/ty/issues/494

There's cases where you can make noisy Literal hints appear, so we can always iterate on it, but this handles like, 98% of the cases in the wild, which is great.

---

_Assigned to @Gankra by @Gankra on 2025-11-10 20:13_

---

_Label `server` added by @Gankra on 2025-11-10 20:13_

---

_Review requested from @carljm by @Gankra on 2025-11-10 20:13_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-10 20:13_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-10 20:13_

---

_Review requested from @sharkdp by @Gankra on 2025-11-10 20:13_

---

_Review requested from @dcreager by @Gankra on 2025-11-10 20:13_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 20:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-10 20:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @sharkdp on `crates/ty_ide/src/inlay_hints.rs`:349 on 2025-11-10 20:27_

Wow, how did I not know that this is legal syntax? :smile:

In general, I think I understand the reasoning behind this syntactic feature (better diffs, easier editing), but for this particular case, I think I would prefer the version that seems more readable to me (it seems unlikely that we add more patterns to this exact case), but definitely no strong opinion.
```suggestion
        Expr::UnaryOp(ExprUnaryOp { op: UnaryOp::UAdd | UnaryOp::USub, operand, .. }) => matches!(**operand, Expr::NumberLiteral(_)),
```


---

_@sharkdp approved on 2025-11-10 20:28_

Thank you!

---

_Merged by @Gankra on 2025-11-10 21:56_

---

_Closed by @Gankra on 2025-11-10 21:56_

---

_Branch deleted on 2025-11-10 21:56_

---

_Label `ty` added by @ntBre on 2025-11-13 19:16_

---
