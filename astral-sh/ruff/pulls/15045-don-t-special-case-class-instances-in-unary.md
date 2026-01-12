```yaml
number: 15045
title: "Don't special-case class instances in unary expression inference"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/unary
created_at: 2024-12-18T15:48:39Z
updated_at: 2024-12-18T19:37:19Z
url: https://github.com/astral-sh/ruff/pull/15045
synced_at: 2026-01-12T15:55:50Z
```

# Don't special-case class instances in unary expression inference

---

_@dcreager_

We have a handy `to_meta_type` that does the right thing for class instances, and also works for all of the other types that are “instances of” something.  Unless I'm missing something, this should let us get rid of the catch-all clause in one fell swoop.

cf #14548

---

_Label `red-knot` added by @dcreager on 2024-12-18 15:48_

---

_Review requested from @carljm by @dcreager on 2024-12-18 15:48_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-18 15:48_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-18 15:48_

---

_Review requested from @sharkdp by @dcreager on 2024-12-18 15:48_

---

_Comment by @github-actions[bot] on 2024-12-18 16:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3208 on 2024-12-18 16:52_

Would using `Type::call_dunder` simplify this even a bit more?

---

_@carljm approved on 2024-12-18 16:53_

Very nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3196 on 2024-12-18 16:53_

nit: I know there's a lot of variants in the `Type` enum, but I'd prefer it if we could explicitly list all the remaining ones in this `match` arm rather than using `_` for the second tuple element. The advantage is that if we add another `Type` variant in the future that we want to special-case in unary expressions (similar to the special-casing we already do for `Type::IntLiteral` and `Type::BooleanLiteral` above), there's no chance of us forgetting to add that special-casing. We'll be forced to explicitly consider whether the new variant should be handled in its own `match` arm or be handled as part of the catch-all fallback case here.

---

_@AlexWaygood approved on 2024-12-18 16:56_

Nice!!

---

_@carljm reviewed on 2024-12-18 17:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3196 on 2024-12-18 17:45_

I'm not so sure explicitly listing all variants is always the best answer. In this case, the handling in this arm really is a generic implementation of the semantics in terms of other type semantics. It should always be correct for _any_ type (even the ones we special case above here), assuming a correct implementation of calling dunder methods (`to_meta_type`, `member`, etc -- all of which _do_ use exhaustive matches and we'll be forced to consider for any new Type variant.) Thus, a special case for a new Type variant here would not be required for correctness, it would just potentially offer more precision. I kind of think it's fine, possibly even good, for that "more precision" to be driven only by actual user needs, and not something we are forced to consider.

But I'm really fine with it either way.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3196 on 2024-12-18 17:50_

Yes, you're right that there isn't a correctness issue here. But I would expect more precision here to be both trivial to accomplish for any new variants and simpler for us to compute. The `IntLiteral()` branches above really involve much less work for us than doing the full lookup for an instance type's dunder method in typeshed or user code (which could involve materialising an MRO!). I don't really see a reason _not_ for us to infer the more precise type whenever possible if it's trivial to do so, and I'd at least like us to be forced to consider whether it would be better to do so

---

_@AlexWaygood reviewed on 2024-12-18 17:50_

---

_@dcreager reviewed on 2024-12-18 18:25_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3208 on 2024-12-18 18:25_

Oh hey look at that!  Yes it would

---

_@dcreager reviewed on 2024-12-18 18:32_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:3196 on 2024-12-18 18:32_

Sounds like there's a -0 vote from Carl and a +1 from Alex, so I went ahead and expanded the clauses out.  We can always put it back to `_` in the future if we find this more unwieldy than the benefit.

---

_Merged by @dcreager on 2024-12-18 19:37_

---

_Closed by @dcreager on 2024-12-18 19:37_

---

_Branch deleted on 2024-12-18 19:37_

---
