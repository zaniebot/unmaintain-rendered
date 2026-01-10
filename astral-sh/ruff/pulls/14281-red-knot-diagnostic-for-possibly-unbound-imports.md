```yaml
number: 14281
title: "[red-knot] Diagnostic for possibly unbound imports"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/possibly-unbound-imports
created_at: 2024-11-11T14:41:39Z
updated_at: 2024-11-11T19:28:40Z
url: https://github.com/astral-sh/ruff/pull/14281
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Diagnostic for possibly unbound imports

---

_Pull request opened by @sharkdp on 2024-11-11 14:41_

## Summary

This adds a new diagnostic when possibly unbound symbols are imported. The `TODO` comment had a question mark, do I'm not sure if this is really something that we want.

This does not touch the un*declared* case, yet.

relates to: #14022

## Test Plan

Updated already existing tests with new diagnostics


---

_Review requested from @carljm by @sharkdp on 2024-11-11 14:41_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-11 14:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-11 14:41_

---

_Comment by @github-actions[bot] on 2024-11-11 14:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-11 14:59_

This is an understand question from my side. When do we add a `possibly-unbound-xy` diagnostic because there are many situations where a value can be unbound. Do we want dedicated diagnostics for members (subscript), imports, variables? What about context managers, patterns, etc or should we rather have a single diagnostic code for all of them?

This discussion is entirely unrelated to this PR and shouldn't prevent it from landing. It's also something we can revisit later.

---

_Label `red-knot` added by @AlexWaygood on 2024-11-11 15:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:107 on 2024-11-11 16:01_

It'll actually get worse when we understand `sys.version_info` branches, because `types.NoneType` only exists on Python 3.10+, and our default Python version is 3.8, which means when we understand `sys.version_info` branches we'll infer `types.NoneType` as always-unbound with the default target version rather than possibly-unbound.

The solution here is really to add a feature to `mdtest` so that we can set `--target-version` to Python 3.10 or higher in this test snippet.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2017 on 2024-11-11 16:59_

I'd go for a slightly shorter error message here

```suggestion
                                    format_args!("Member `{name}` of module `{module_name}` is possibly unbound"),
```

---

_@AlexWaygood approved on 2024-11-11 17:02_

I agree with adding a diagnostic for this. "The member might be unbound" _does_ feel to me like a distinct thing to "the module just doesn't have this member", so I guess I agree with this being a separate error code... but I also absolutely see @MichaReiser's point about this leading to the number of error codes exploding a bit :/

---

_Comment by @carljm on 2024-11-11 17:58_

> "The member might be unbound" _does_ feel to me like a distinct thing to "the module just doesn't have this member", so I guess I agree with this being a separate error code... but I also absolutely see @MichaReiser's point about this leading to the number of error codes exploding a bit :/

I understood something different from Micha's question. I didn't understand it to be about `unbound-x` vs `possibly-unbound-x` (I think these must be different error codes, and 2x for unbound error codes is a manageable level of "explosion"), but rather about how finely we divide the many different possible `x`. With this PR, we now have `unresolved-reference` and `possibly-unbound-reference` and now `unresolved-import` and `possibly-unbound-import`.

I think this is an area where it's worth looking at what other type checkers do. I lean towards separate error codes, and my understanding from mypy is that users seem to demand rather finely-grained error codes. It's also not clear to me what is the concrete _cost_ of finer-grained error codes. It clearly provides more functionality, and the cost of "longer reference docs", while real, doesn't seem to outweigh the user flexibility.

Imports are different in that an unbound-import error may often come from code you don't control. I think another useful guide may be "which things raise different errors at runtime", which would suggest separate codes: unbound name references, nonexistent attributes, missing imports, and missing mapping keys all raise different exception types at runtime.

---

_@carljm approved on 2024-11-11 17:59_

Looks great, thank you!

---

_Comment by @MichaReiser on 2024-11-11 18:25_

> I think this is an area where it's worth looking at what other type checkers do. I lean towards separate error codes, and my understanding from mypy is that users seem to demand rather finely-grained error codes. It's also not clear to me what is the concrete cost of finer-grained error codes. It clearly provides more functionality, and the cost of "longer reference docs", while real, doesn't seem to outweigh the user flexibility.

The cost here is mainly:

* Each rule will come with some boilerplate and needs documenting. This is a one off cost but also leads to more code.
* Users ideally don't toggle rules at such a fine granular level and instead use e.g. a rule selector. But every rule adds some cognitive overhead (e.g. the list of rules becomes longer and more difficult to search, it's harder for users to find the rules they care about). 

---

_Comment by @sharkdp on 2024-11-11 19:17_

Thanks for the discussion about rule names. I'll leave the fine-grained rule name for now, assuming that we will do a overhaul at some point to enforce consistency across all rules. But let me know if you think something should be changed for this specific rule.

---

_Merged by @sharkdp on 2024-11-11 19:26_

---

_Closed by @sharkdp on 2024-11-11 19:26_

---

_Branch deleted on 2024-11-11 19:26_

---
