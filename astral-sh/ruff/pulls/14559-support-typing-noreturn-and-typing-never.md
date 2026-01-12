```yaml
number: 14559
title: "Support `typing.NoReturn` and `typing.Never`"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: typing-no-return-never
created_at: 2024-11-23T17:07:38Z
updated_at: 2024-11-26T09:49:54Z
url: https://github.com/astral-sh/ruff/pull/14559
synced_at: 2026-01-12T15:55:48Z
```

# Support `typing.NoReturn` and `typing.Never`

---

_@Glyphack_

Fix #14558 
## Summary

- Add `typing.NoReturn` and `typing.Never` to known instances and infer them as `Type::Never`
- Add `is_assignable_to` cases for `Type::Never`

I skipped emitting diagnostic for when a function is annotated as `NoReturn` but it actually returns.

## Test Plan

Added tests from
https://github.com/python/typing/blob/main/conformance/tests/specialtypes_never.py
except from generics and checking if the return value of the function and the annotations match.

---

_Comment by @github-actions[bot] on 2024-11-23 17:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @AlexWaygood on 2024-11-23 18:10_

---

_@Glyphack reviewed on 2024-11-24 13:50_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types.rs`:403 on 2024-11-24 13:50_

I'm not sure if this will be useful for anything other than showing the type correctly to the user on hover. Without adding this field we show both `NoReturn` and `Never` as `Never`.
But this makes you type few more characters on pattern matching and when creating the type(The type creation can be simplified by making a function that would return the `Type::Never(NeverType::Never)`.)

---

_Marked ready for review by @Glyphack on 2024-11-24 13:54_

---

_Review requested from @carljm by @Glyphack on 2024-11-24 13:54_

---

_Review requested from @MichaReiser by @Glyphack on 2024-11-24 13:54_

---

_Review requested from @AlexWaygood by @Glyphack on 2024-11-24 13:54_

---

_Review requested from @sharkdp by @Glyphack on 2024-11-24 13:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4643 on 2024-11-24 19:33_

This means that we panic if a user writes something like this and then asks red-knot to check their code:

```py
import typing

x: typing.Never[int]
```

Backtrace:

```
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/infer.rs:4635:17:
internal error: entered unreachable code: These types cannot be parametrized and have their own case
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Rayon: detected unexpected panic; aborting
zsh: abort      cargo run -p red_knot -- --current-directory ../experiment
```

The annotation `typing.Never` is invalid but we shouldn't panic if the user writes something like this. Instead, we should emit a diagnostic and infer `Unknown`.

So it seems like this branch is pretty reachable ;)

---

_@AlexWaygood had review dismissed on 2024-11-24 19:34_

Thanks! Not a full review yet, but one important thing I noticed:

---

_@Glyphack reviewed on 2024-11-24 20:22_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4643 on 2024-11-24 20:22_

Thanks! I don't know why at the time I thought no branches go to this. Added 
```error: [invalid-type-parameter] "Type `typing.Never` expected no type parameter"```
diagnostic and added a test for this case.

---

_@AlexWaygood reviewed on 2024-11-24 20:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4643 on 2024-11-24 20:24_

Great, thank you!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:33 on 2024-11-24 20:26_

Let's use the specified language of "assignable to", not the unspecified and informal "compatible with".

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:403 on 2024-11-24 20:30_

My feeling is that we do not need to maintain an internal distinction between `Never` and `NoReturn`, as types. (We should distinguish between them as known instances in the `typing` module, as you do.) They are the same type, and it's not even clear to me that it results in better UX to distinguish them. `NoReturn` is just an alternate form for function return annotations. The inferred type from calling a function annotated to return `NoReturn` is IMO actually better revealed as `Never` than `NoReturn`, because the term `NoReturn` makes no sense outside the immediate return annotation context.

(Let's make sure @AlexWaygood doesnt have a strong feeling otherwise, before you go through the work of removing this enum.)

---

_@carljm reviewed on 2024-11-24 20:30_

Also not a full review yet, just a couple things I noticed in passing. 

---

_@AlexWaygood reviewed on 2024-11-24 20:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:403 on 2024-11-24 20:35_

> Let's make sure @AlexWaygood doesnt have a strong feeling otherwise, before you go through the work of removing this enum.

I agree! I had thoughts along the same lines, though I hadn't written them up yet. Mypy and pyright do the same, IIRC

---

_@dhruvmanila reviewed on 2024-11-25 04:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4579 on 2024-11-25 04:08_

nit: I think we can skip passing the `slice` if we pass the `subscript` as it can be fetched from the subscript directly

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:16 on 2024-11-25 08:15_

I think this should be a separate test that ends here. If you were to actually run this code, the program would never return from the `stop()` call.

Everything after this line can be considered unreachable code from the perspective of static analysis. Which also means we might not (should not) emit any diagnostics, including `reveal_type`-information. I guess it's even questionable as to whether or not the `reveal_type` in this line should emit a diagnostic. You would not see any result at runtime. Existing type checkers still reveal the type for the `stop()` call, but nothing beyond.

Similar reasoning applies to some of the tests below, I think.

---

_@sharkdp reviewed on 2024-11-25 08:15_

---

_@Glyphack reviewed on 2024-11-25 08:40_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:16 on 2024-11-25 08:40_

My intention was to test the revealed type only. You're right this test will break when the behavior for Never is added.
I will move this call and other `reveal_types` that can stop the type checking to a function.

> I think this should be a separate test that ends here.

Are you suggesting to implement this behavior in this PR? Otherwise I can add a todo comment here and work on it in a follow up PR.

---

_@sharkdp reviewed on 2024-11-25 09:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:16 on 2024-11-25 09:28_

> Are you suggesting to implement this behavior in this PR?

Ah, no — sorry. I was merely suggesting that we split this large test into multiple smaller tests. And take care not to have any test assertions (`# revealed: ` / `# error: `) in lines following a call to a function that returns `Never`/`NoReturn`. To make sure this test will work in the future once we add unreachable-code analysis.

Writing smaller self-consistent tests would also be beneficial if we ever need to debug an assertion failure in this file.

---

_Comment by @carljm on 2024-11-25 16:01_

> I skipped emitting diagnostic for when a function is annotated as `NoReturn` but it actually returns.

This is correct to skip in this PR; there shouldn't be any special-casing of such a check, it will just fall out naturally from checking return type assignability in general, since nothing (except for `Never`) is assignable to `Never`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4589 on 2024-11-25 18:23_

if you do this then you can avoid the `.as_ref()` call on line 4611:

```suggestion
        let parameters = &*subscript.slice;
```

---

_@AlexWaygood reviewed on 2024-11-25 18:23_

---

_@Glyphack reviewed on 2024-11-25 18:25_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types.rs`:403 on 2024-11-25 18:25_

Thank you both. I reverted this one.

---

_@Glyphack reviewed on 2024-11-25 18:26_

---

_Review comment by @Glyphack on `crates/red_knot_python_semantic/src/types/infer.rs`:4589 on 2024-11-25 18:26_

Great I did not know about this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/never.md`:5 on 2024-11-25 18:39_

```suggestion
`NoReturn` is used to annotate functions that never return (they always raise an exception or exit the process).
`Never` is the bottom type, and represents the empty set of Python objects. These two annotations can be used
interchangeably.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:759 on 2024-11-25 21:22_

I think this case is a) wrong (because `Never` should be assignable to `Never`) and b) never reached, due to the `is_equivalent_to` check that takes precedence above.

There is a test verifying that `Never` is actually assignable to `Never`.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:760 on 2024-11-25 21:24_

And I think this case should also not be necessary, because we fallback to `is_subtype_of`, which already contains this same case for `Never`. Removing this line and the previous one, all tests still pass.
```suggestion
```

---

_@carljm approved on 2024-11-25 21:26_

Looks great, thank you! Just a few small tweaks which I'll push, and then merge.

---

_Merged by @carljm on 2024-11-25 21:37_

---

_Closed by @carljm on 2024-11-25 21:37_

---

_Branch deleted on 2024-11-26 09:49_

---
