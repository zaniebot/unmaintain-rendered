```yaml
number: 19069
title: "[ty] Fix panic for attribute expressions with empty value"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-738
created_at: 2025-07-01T13:12:51Z
updated_at: 2025-07-09T06:46:35Z
url: https://github.com/astral-sh/ruff/pull/19069
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Fix panic for attribute expressions with empty value

---

_Pull request opened by @sharkdp on 2025-07-01 13:12_

## Summary

closes https://github.com/astral-sh/ty/issues/738

## Test Plan

Added corpus test

---

_Label `ty` added by @sharkdp on 2025-07-01 13:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-01 13:13_

This is probably not the best (and not the broadest) fix here. Maybe @mtshiba could have a look at this if you find the time?

---

_@sharkdp reviewed on 2025-07-01 13:13_

---

_Comment by @github-actions[bot] on 2025-07-01 13:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~334MB
+     memo fields = ~304MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

sympy (https://github.com/sympy/sympy)
-     memo fields = ~1399MB
+     memo fields = ~1538MB

```
</details>


---

_@mtshiba reviewed on 2025-07-07 18:24_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-07 18:24_

Sorry for the late review. This fix looks good!

One thing I am wondering about is our current handling of (syntactically) invalid place expressions.
The parser seems to construct the following element for `[.a.`, for example:

```
Attribute(ExprAttribute {
    node_index: AtomicNodeIndex(3),
    range: 1..3,
    value: Name(ExprName { node_index: AtomicNodeIndex(4), range: 1..1, id: Name(""), ctx: Invalid }),
    attr: Identifier { id: Name("a"), range: 2..3, node_index: AtomicNodeIndex(5) },
    ctx: Load
})
```

This is also not a valid expression and we can immediately return `Unbound`. Rather, this seems to cause a panic, but it doesn't. Because it is recorded as a place in the `UseDefMap`. The inner name is invalid, but the attribute is in the load context. Not harmful, but a useless record.

https://github.com/astral-sh/ruff/blob/f15f930053a829d366e55e2dee8c454f20f66dcd/crates/ty_python_semantic/src/semantic_index/builder.rs#L1945-L1962

It might be better to validate that the inner expressions are also valid when building the `SemanticIndex`, or to let the parser propagate the invalid context to the outer expressions.

---

_@sharkdp reviewed on 2025-07-08 08:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-08 08:58_

@dhruvmanila Would you mind also having a look at this, in particular the suggestion here to do part of the work in the parser?

---

_@dhruvmanila reviewed on 2025-07-08 09:59_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-08 09:59_

> It might be better to validate that the inner expressions are also valid when building the `SemanticIndex`, or to let the parser propagate the invalid context to the outer expressions.

I think it makes sense that if an inner expression is invalid then the entire attribute expression is marked as invalid by the parser. This would then be automatically excluded by the semantic index builder. We do related changes using the [`helpers::set_expr_context`](https://github.com/astral-sh/ruff/blob/1ddda241f69d6e1aa651d279983a9738ddb2f892/crates/ruff_python_parser/src/parser/helpers.rs#L9-L9) function.

Do we only need to account for the top-level invalid expression or any nested invalid expressions as well? For example, in `.a`, it's only the top level `value` expression of the attribute expression that's invalid but in `foo(1+).bar`, it's the inner expression (`+1`) that's invalid instead of the outer expression `foo(...)` and the outer expression corresponds to the `value` field of an attribute expression. I'm assuming it's the former?

---

_@mtshiba reviewed on 2025-07-08 17:01_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-08 17:01_

> Do we only need to account for the top-level invalid expression or any nested invalid expressions as well? For example, in .a, it's only the top level value expression of the attribute expression that's invalid but in foo(1+).bar, it's the inner expression (+1) that's invalid instead of the outer expression foo(...) and the outer expression corresponds to the value field of an attribute expression. I'm assuming it's the former?

The only expressions that should have invalid context propagated are those that are simple enough that `PlaceExpr::try_from` succeeds.
A complex expression like `f(1+).bar` is not recorded as a place in the `UseDefMap`, so the outer attribute `f(...).bar` can be used as the load context. 

---

_@dhruvmanila reviewed on 2025-07-09 04:27_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-09 04:27_

That seems a bit inconsistent because the parser shouldn't care whether the expression can be constructed by `PlaceExpr` or not. And, I don't think it really matters, right? Like, if there are any invalid expressions nested in the `ExprAttribute`, then the entire `ExprAttribute` is invalid and the parser shouldn't special case only specific expressions. But, if special casing is important, then I'd prefer to have this logic in ty and not in the parser.

---

_@MichaReiser reviewed on 2025-07-09 06:00_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-09 06:00_

Is changing the context in the parser significantely simpler than doing it in ty? If not, I'd suggest doing it in ty.

---

_@dhruvmanila reviewed on 2025-07-09 06:21_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-09 06:21_

I think it won't be that simple change in the parser because we'd need to get this information (`ExprContext::Invalid`) up the AST to the `ExprAttribute` which could possibly be achieved by storing it in `ParsedExpr` (`is_invalid: bool` field) and using it in `parse_attribute_expression`. But, yeah, it might be simpler to do it in ty instead.

---

_Marked ready for review by @sharkdp on 2025-07-09 06:22_

---

_Review requested from @carljm by @sharkdp on 2025-07-09 06:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-09 06:22_

---

_Review requested from @dcreager by @sharkdp on 2025-07-09 06:22_

---

_@sharkdp reviewed on 2025-07-09 06:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5683 on 2025-07-09 06:24_

Thanks everyone. If this really only causes unnecessary work, I'm not sure it's worth doing something here at all. I'll move this PR to in-review to fix the original crash. It comes up easily in the LSP/playground when editing code.

---

_@AlexWaygood approved on 2025-07-09 06:40_

---

_Merged by @sharkdp on 2025-07-09 06:46_

---

_Closed by @sharkdp on 2025-07-09 06:46_

---

_Branch deleted on 2025-07-09 06:46_

---
