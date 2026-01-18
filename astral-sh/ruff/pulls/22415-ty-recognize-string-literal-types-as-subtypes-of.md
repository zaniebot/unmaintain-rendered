```yaml
number: 22415
title: "[ty] Recognize string-literal types as subtypes of `Sequence[Literal[chars]]`"
type: pull_request
state: merged
author: jhartum
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: feat/string-literal-sequence-subtype-v2
created_at: 2026-01-06T09:34:15Z
updated_at: 2026-01-18T17:43:44Z
url: https://github.com/astral-sh/ruff/pull/22415
synced_at: 2026-01-18T18:15:41Z
```

# [ty] Recognize string-literal types as subtypes of `Sequence[Literal[chars]]`

---

_@jhartum_

## Summary

Implements https://github.com/astral-sh/ty/issues/2128: `Literal["abba"]` is now recognized as a subtype of `Sequence[Literal["a", "b"]]`.

## Changes

- **Add `KnownClass::Sequence`** - New known class for `typing.Sequence` (an ABC, not a protocol)
- **Extend `has_relation_to_impl`** - When checking if a string literal is a subtype of `Sequence[X]`, verify that each unique character in the string is a subtype of `X`
- **Add mdtests** - Cover positive cases, negative cases, and edge cases

## Example

```python
from collections.abc import Sequence
from typing import Literal

def func(tags: Sequence[Literal["a", "b"]]) -> None:
    pass

func("abba")  # Now OK - was incorrectly flagged as error
```

## Implementation Details

The implementation in `has_relation_to_impl`:
1. First tries the standard `str` fallback (for cases like `Sequence[str]`)
2. If that fails and the target is `Sequence[X]`, extracts unique characters from the string literal
3. Verifies each character is a subtype of `X` using `when_all` to properly accumulate constraints
4. Returns the combined constraint set

Edge cases handled:
- Empty strings (always valid)
- Unicode characters
- Multi-char literals in element type (correctly rejected - `Literal["ab"]` ≠ `Literal["a", "b"]`)

## Test Plan

- Added mdtests in `is_subtype_of.md`
- All existing mdtests pass

Closes https://github.com/astral-sh/ty/issues/2128

---

_Review requested from @carljm by @jhartum on 2026-01-06 09:34_

---

_Review requested from @AlexWaygood by @jhartum on 2026-01-06 09:34_

---

_Review requested from @sharkdp by @jhartum on 2026-01-06 09:34_

---

_Review requested from @dcreager by @jhartum on 2026-01-06 09:34_

---

_Label `ty` added by @MichaReiser on 2026-01-06 09:35_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 09:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-06 09:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14496 diagnostics
+ Found 14497 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:4840 on 2026-01-06 09:42_

This isn't correct — `Sequence` is an ABC, not a protocol. Among other things, that means that you can use it as the second argument to isinstance() or issubclass(). (You can see lots of new diagnostics in the ecosystem caused by this currently.)

---

_@AlexWaygood reviewed on 2026-01-06 09:43_

Thanks! I haven't looked in depth yet, just one thing I spotted 

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2026-01-06 09:46_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 10:18_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-return-type` | 2 | 0 | 4 |
| `invalid-argument-type` | 2 | 1 | 2 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **4** | **3** | **13** |


**[Full report with detailed diff](https://26b7efd1.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://26b7efd1.ty-ecosystem-ext.pages.dev/timing))



---

_@jhartum reviewed on 2026-01-06 10:40_

Thanks for catching this! Fixed.

---

_Comment by @jhartum on 2026-01-06 10:40_

> Thanks! I haven't looked in depth yet, just one thing I spotted

Thanks for catching this! Fixed.

---

_Renamed from "feat(ty): Recognize string literals as subtypes of Sequence[Literal[chars]]" to "[ty] Recognize string-literal types as subtypes of `Sequence[Literal[chars]]`" by @AlexWaygood on 2026-01-06 14:22_

---

_@AlexWaygood approved on 2026-01-06 14:48_

Thanks!

---

_Comment by @codspeed-hq[bot] on 2026-01-06 14:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **not alter performance**




`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



---

<sub>Comparing <code>jhartum:feat/string-literal-sequence-subtype-v2</code> (528ded0) with <code>main</code> (57c98a1)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/jhartum%3Afeat%2Fstring-literal-sequence-subtype-v2?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/jhartum%3Afeat%2Fstring-literal-sequence-subtype-v2?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Comment by @AlexWaygood on 2026-01-06 14:59_

Uff. I think I know what I screwed up there.

---

_@MichaReiser reviewed on 2026-01-06 15:31_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/relation.rs`:986 on 2026-01-06 15:31_

Can we use `to_compact_string` here to avoid allocations?

---

_@AlexWaygood reviewed on 2026-01-06 15:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:986 on 2026-01-06 15:35_

thanks. I thought I tried that previously, but apparently not.

---

_Comment by @MichaReiser on 2026-01-06 17:09_

We may have to do a profiling run on Altair to see where we spend more time now. I suspect we run into lock contention because we keep re-interning the same characters over and over again (as string literals). It might be worth adding some debug logging to count how many `StringLiteral` types we created by character to see if there are any that stand out

---

_Comment by @AlexWaygood on 2026-01-06 17:23_

The optimisations I pushed got the time on the multithreaded benchmark down from 2.3s to 1.8s, so it's much better than it _was_... but a 23% performance regression is _obviously_ far from ideal...

I have the same suspicion you do about the cause of the regression, but if that is the cause, what do we do about it? Can you think of any ways to avoid reinterning the same characters over and over here? I already pushed a change to only call `StringLiteralType::new()` on the characters after they've been deduplicated, to try to avoid the lock contention issues (and it did lead to a big speedup, but there's still an overall regression).

---

_@carljm reviewed on 2026-01-06 17:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/relation.rs`:973 on 2026-01-06 17:24_

I suspect that by far the most effective optimization for this PR would be if we could find ways to narrow down the `_` in this match. Right now we are doing all this work to construct a new sequence-of-characters type anytime we compare a string literal type with anything other than `LiteralString` or `NominalInstance(str)` -- and this could happen quite often e.g. with construction of unions, not necessarily with explicit assignment attempts.

Using `_` here is nice and simple and tempting because it should always be correct to try this delegation -- but can we narrow it down further without losing any correct results? Like for instance, is the new sequence type ever going to be a subtype of anything that isn't a nominal instance type or a protocol?

Or another approach: what if we want this subtype relationship for assignability, but we actually don't care about simplifying a union of `Literal["abba"] | Sequence["a", "b"]`? We could skip this branch if `relation` is redundant-with, and I suspect that would eliminate most of the regression.

Ultimately I think understanding this subtype relationship is pretty low priority (no other type checker does it), so if we can't do it without regression, we should consider not doing it at all.

---

_@AlexWaygood reviewed on 2026-01-06 17:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:973 on 2026-01-06 17:28_

> no other type checker does it

Ah, I wasn't aware of that! I believed that they did, but you're right.

> if we can't do it without regression, we should consider not doing it at all.

yeah, agreed

---

_@AlexWaygood reviewed on 2026-01-06 17:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:973 on 2026-01-06 17:58_

Restricting the branch so that it was only `(Type::StringLiteral(value), Type::NominalInstance(...))` did not have any impact from my local measurements (though I applied it anyway; it makes sense).

Switching it so that the relation only applies for assignability had a huge difference, however.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/relation.rs`:1045 on 2026-01-06 18:05_

I think if we do it for assignability we should probably do it for subtyping as well? For fully static types I think it's important that assignability and subtyping stay equivalent.

I expect that specifically excluding is-redundant-with here would have the same impact, since subtyping is rarely used.

---

_@carljm reviewed on 2026-01-06 18:05_

---

_@AlexWaygood reviewed on 2026-01-06 18:11_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:1045 on 2026-01-06 18:11_

> For fully static types I think it's important that assignability and subtyping stay equivalent.

Why is assignability/subtyping consistency more important than assignability/redundancy consistency for fully static types? Because we use the subtyping relation in the generics solver, whereas we only use redundancy for simplifying unions?

Making the change you suggest here would introduce the first instance where the redundancy type relation would be less forgiving than the subtyping relation. I think it makes it much harder to reason about if we go there. And I'd have to go and rewrite a lot of this docstring: https://github.com/astral-sh/ruff/blob/ab1ac254d9750d23e489331e2a5c6ea08172632b/crates/ty_python_semantic/src/types/relation.rs#L87-L141

---

_@AlexWaygood reviewed on 2026-01-06 18:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/relation.rs`:1045 on 2026-01-06 18:15_

The latest version of this PR has no performance regression, so this is the last remaining question to resolve before it can be landed: https://codspeed.io/astral-sh/ruff/branches/jhartum%3Afeat%2Fstring-literal-sequence-subtype-v2?utm_source=github&utm_medium=comment-v2&utm_content=button

---

_Review requested from @carljm by @AlexWaygood on 2026-01-06 18:41_

---

_@carljm reviewed on 2026-01-07 18:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/relation.rs`:1045 on 2026-01-07 18:52_

> Why is assignability/subtyping consistency more important than assignability/redundancy consistency for fully static types? Because we use the subtyping relation in the generics solver, whereas we only use redundancy for simplifying unions?

Yes, because we use subtyping in various places where we need a definitive and theoretically consistent answer whether one type subsumes another, whereas redundancy is used only in union simplification. There is no correctness requirement that union simplification always remove types that could potentially be safely removed. In fact we have open issues already suggesting other cases where we might want union simplification to avoid removing some technically-redundant types. So I don't think the invariant that redundancy be "more forgiving" than subtyping is important for any particular reason, and I don't expect that invariant to last long anyway.

Maybe in the future this will demand a larger rewrite of that doc-comment (that doesn't say things like "sits between"), but I don't think the specific change here would demand a rewrite, just a brief aside about one particular pragmatic special-case exception.

---

_Comment by @MichaReiser on 2026-01-08 15:15_

The 5-10% memory usage increase is a bit concerning

Maybe a feature we should get back to in the future and drop for now?

---

_Comment by @bxff on 2026-01-09 17:44_

 I think this should help with the memory regression. The issue is we're doing expensive character processing for *every* `StringLiteral` vs `NominalInstance` comparison, even when it's clearly not a sequence type.

Here's a quick fix that adds a cheap MRO check before any allocations:

```diff
diff --git a/crates/ty_python_semantic/src/types/relation.rs b/crates/ty_python_semantic/src/types/relation.rs
index ab84708b65..e832ae59bf 100644
--- a/crates/ty_python_semantic/src/types/relation.rs
+++ b/crates/ty_python_semantic/src/types/relation.rs
@@ -3,6 +3,7 @@ use rustc_hash::FxHashSet;
 
 use crate::place::{DefinedPlace, Place};
 use crate::types::builder::RecursivelyDefined;
+use crate::types::class_base::ClassBase;
 use crate::types::constraints::{IteratorConstraintsExtension, OptionConstraintsExtension};
 use crate::types::enums::is_single_member_enum;
 use crate::types::{
@@ -1047,6 +1048,22 @@ impl<'db> Type<'db> {
                     return ConstraintSet::from(true);
                 }
 
+                if let Some(sequence_class) = KnownClass::Sequence.try_to_class_literal(db) {
+                    let is_sequence_subclass =
+                        sequence_class.iter_mro(db, None).any(|base| match base {
+                            ClassBase::Class(base_class) => {
+                                base_class.class_literal(db).0 == other_class.class_literal(db).0
+                            }
+                            _ => false,
+                        });
+
+                    if !is_sequence_subclass {
+                        return ConstraintSet::from(false);
+                    }
+                } else {
+                    return ConstraintSet::from(false);
+                }
+
                 let chars: FxHashSet<char> = value.value(db).chars().collect();
```

This cuts the memory overhead dramatically by only doing character interning when the target class is actually in the Sequence hierarchy. On the `attrs` benchmark: **+61 MB → +10 MB** (down to ~2.5% overhead from 16%). All 308 mdtests pass.

---

_Converted to draft by @AlexWaygood on 2026-01-18 16:33_

---

_Marked ready for review by @AlexWaygood on 2026-01-18 17:40_

---

_Comment by @AlexWaygood on 2026-01-18 17:42_

Thanks @bxff! I applied a variant of that and it did indeed solve the memory-usage regression.

This PR now applies consistent rules for assignability/subtyping/redundancy, does not add any new Salsa caching, does not have any reported memory-usage regressions, and does not have any reported performance regressions. So I think it's good to go.

Thanks @jhartum!! Sorry that this one turned out to be a bit more complicated than we initially expected.

---

_Merged by @AlexWaygood on 2026-01-18 17:43_

---

_Closed by @AlexWaygood on 2026-01-18 17:43_

---
