```yaml
number: 12986
title: "[red-knot] Track root cause of why a type is inferred as `Unknown`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/unknown-kind
created_at: 2024-08-19T11:00:26Z
updated_at: 2025-05-07T15:23:59Z
url: https://github.com/astral-sh/ruff/pull/12986
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Track root cause of why a type is inferred as `Unknown`

---

_Pull request opened by @AlexWaygood on 2024-08-19 11:00_

## Summary

This PR adds detailed information to our type inference so we can track exactly why a symbol has been inferred as `Unknown`. This allows us to restore the `unresolved-import` red-knot lint to the level of accuracy it had prior to https://github.com/astral-sh/ruff/commit/a9847af6e89c593dcb49023b5e88ba7f4a84a61a, because we can now distinguish between `Unknown` types that were caused by unresolved imports and other kinds of `Unknown` types. The approach is similar to [the one mypy uses](https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/types.py#L187-L215).

## Test Plan

There are two ways in which this PR is tested:
1. The assertion in the benchmark is changed: six spurious "Unresolved import" diagnostics go away, meaning the number of diagnostics drops from 34 to 28. The only remaining "unresolved import" diagnostic is emitted on [this line](https://github.com/python/cpython/blob/e077b201f49a6007ddad7c1b6e3069a037b6d952/Lib/tomllib/_parser.py#L7), which is because we don't understand `*` imports yet, and `Iterable` is defined in the typeshed stub for `collections.abc` [here](https://github.com/python/typeshed/blob/937270df0c25dc56a02f7199f1943fdb7d47aa9d/stdlib/collections/abc.pyi#L1).
2. A test is unskipped in `red_knot_workspace/src/lint.rs` asserting that a project structure (where the `foo` module does not exist) like this only triggers an "unresolved import" diagnostic in `a.py`, and not in `b.py`:
   - `/src/a.py`: `import foo as foo`
   - `/src/b.py`: `from a import foo`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-19 11:00_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-19 11:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-19 11:00_

---

_@AlexWaygood reviewed on 2024-08-19 11:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:257 on 2024-08-19 11:01_

I derived `Ord` and `PartialOrd` here because the `Type` enum derives them, but I'm not sure it makes sense to do so. I don't really understand why the `Type` enum derives them in the first place.

---

_@AlexWaygood reviewed on 2024-08-19 11:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:314 on 2024-08-19 11:03_

This covers quite a lot. If the intention is to only ever emit diagnostics after all types have been inferred, we may need to split this into several variants, so that we can emit different error messages (under different codes) for different kinds of errors. On the other hand, we could consider collecting some diagnostics as part of the `TypeInferenceBuilder`, which would mean we wouldn't need so much granularity here.

---

_Comment by @github-actions[bot] on 2024-08-19 11:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-19 11:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:257 on 2024-08-19 11:38_

Yeah, me neither. I would remove `Ord` from `Type`

---

_@MichaReiser reviewed on 2024-08-19 11:40_

How does this new type work when unifying or intersecting types? Do we keep all different variants?

---

_Comment by @AlexWaygood on 2024-08-19 11:48_

> How does this new type work when unifying or intersecting types? Do we keep all different variants?

Good question. Currently yes, you could end up with a union such as `int | Unknown(UnknownTypeKind::UnresolvedImport) | Unknown(UnknownTypeKind::TypeError)`.

I suppose we should flatten them into a single `Unknown` member. But then the question becomes: what `kind` of `Unknown` should it be? Maybe `UnknownTypeKind::SecondOrder`, since it results from the union of two `Unknown` elements?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:257 on 2024-08-19 12:14_

I removed the `PartialOrd` and `Ord` implementations in https://github.com/astral-sh/ruff/pull/12986/commits/ff6b1484e9dc743da2763da0dd82864c7b347472

---

_@AlexWaygood reviewed on 2024-08-19 12:14_

---

_Comment by @MichaReiser on 2024-08-20 06:56_

I would love to wait with this PR to get @carljm opinion. I feel uncertain about the type's definition because there are no "obvious" definitions for unification and intersection (unless we make it a bit set, at least for unification). 

It's further unclear if we still need to distinguish between the two types, now that the check for unresolved imports moves into the type checker. 

I had a quick look at pyright and from what i understand is that it uses `Unknown` for unresolved imports but it only has a single `Unknown` type.

https://github.com/microsoft/pyright/blob/52a47010b9db2ad6801b4b35d987cb9f4c923c18/packages/pyright-internal/src/analyzer/typeEvaluator.ts#L18629-L18676

---

_Comment by @AlexWaygood on 2024-08-20 07:01_

> I would love to wait with this PR to get @carljm opinion. I feel uncertain about the type's definition because there are no "obvious" definitions for unification and intersection

Agreed, I'm definitely not going to merge until we've heard Carl's thoughts here! There's no rush on this.

---

_Comment by @AlexWaygood on 2024-08-20 11:58_

> I feel uncertain about the type's definition because there are no "obvious" definitions for unification and intersection (unless we make it a bit set, at least for unification).

FWIW, I think simplifying unions is inevitably going to become more complex than what we currently have, whether we go with this PR or not, because of the fact that we will want to simplify `Literal[True] | Literal[False]` to `Instance(builtins.bool)`, and similarly for other [sealed types](https://github.com/astral-sh/ty/issues/245) such as enums.

> It's further unclear if we still need to distinguish between the two types, now that the check for unresolved imports moves into the type checker.

Hmm, yeah, not sure. I'll try to pull out some of the improvements here into a separate PR that doesn't introduce the new `UnknownTypeKind` enum, so that we can evaluate in isolation exactly whether it improves anything for us right now.

---

_@AlexWaygood reviewed on 2024-08-21 14:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:461 on 2024-08-21 14:28_

Following https://github.com/astral-sh/ruff/commit/ecd9e6a650ef428be67bb0e28cb0c52d27eb2895, the key benefit of this PR is now that this test can be unskipped. This improvement is also reflected in the fact that six spurious "unresolved import" diagnostics go away in the benchmarks.

---

_Comment by @codspeed-hq[bot] on 2024-08-21 14:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/unknown-kind)

### Merging astral-sh/ruff#12986 will **degrade performances by 5.41%**

<sub>Comparing <code>alex/unknown-kind</code> (698de48) with <code>main</code> (ecd9e6a)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex/unknown-kind)._

### Benchmarks breakdown

|     | Benchmark | `main` | `alex/unknown-kind` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[numpy/globals.py]` | 170.5 µs | 180.3 µs | -5.41% |


---

_Comment by @carljm on 2024-08-22 01:31_

I'm open to the idea that we may need to do this eventually, but there are complexity and efficiency disadvantages to smuggling additional context through the type system this way, so I'd rather not do it until it's really clear exactly why we need it. And I don't think that's currently clear, given that the issue with the unresolved-imports check in main branch that this PR fixes is also easily fixable with a much smaller change: https://github.com/astral-sh/ruff/pull/13007

I don't think we should ever need this for the sake of deciding whether or not to emit a diagnostic. `Unknown` may _result_ from a type error, but I don't think its existence should ever create a type error. The fact that main branch currently treats an import of any `Unknown` typed value as an error is IMO not correct, and is fixed by the change linked above.

I would be interested in knowing more about precisely how mypy uses its extra information about the origin of `Any`.

---

_Comment by @AlexWaygood on 2024-08-22 11:37_

> I don't think we should ever need this for the sake of deciding whether or not to emit a diagnostic. `Unknown` may _result_ from a type error, but I don't think its existence should ever create a type error. The fact that main branch currently treats an import of any `Unknown` typed value as an error is IMO not correct, and is fixed by the change linked above.

Thanks, I think this is correct; treating `Unknown` as a type error goes against the spirit of gradual typing. We need to catch the error before we transform the `Unbound` into `Unknown`. My mistake.

---

_Comment by @AlexWaygood on 2024-08-22 13:02_

> I would be interested in knowing more about precisely how mypy uses its extra information about the origin of `Any`.

Much of it seems to be used for precise diagnostics when strict mode is enabled. Mypy `--strict` enables several checks that prevent implicit `Any` types from accidentally percolating through your code. So mypy wants to be able to distinguish between `Any`s that result from missing generic arguments, unannotated variables, and objects explicitly annotated as `Any` (for example).

E.g. [here](https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/checker.py#L2939-L2951) the type of `Any` is checked by the [`has_any_from_unimported_type()`](https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/typeanal.py#L2352-L2370) function in order to implement mypy's optional error code [`[no-any-unimported]`](https://mypy.readthedocs.io/en/stable/error_code_list2.html#check-that-types-have-no-any-components-due-to-missing-imports-no-any-unimported).

---

_Comment by @AlexWaygood on 2024-08-22 13:06_

I still think there's a good chance that we'll need something like this eventually, but I agree that with the fixes in astral-sh/ruff#13055, there are few immediate benefits to this right now. Thanks both!

---

_Closed by @AlexWaygood on 2024-08-22 13:06_

---

_Branch deleted on 2024-08-22 13:07_

---

_Comment by @carljm on 2024-08-22 16:52_

> Much of it seems to be used for precise diagnostics when strict mode is enabled.

Yeah, that makes sense. I agree, we'll probably need something like this for the same reason, when we want to have an Any-forbidding strict mode and emit useful diagnostics for it.

---
