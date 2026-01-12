```yaml
number: 20377
title: "[ty] Heterogeneous unpacking support for unions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/tuplespec-union
created_at: 2025-09-14T16:46:21Z
updated_at: 2025-10-15T18:30:05Z
url: https://github.com/astral-sh/ruff/pull/20377
synced_at: 2026-01-12T15:57:00Z
```

# [ty] Heterogeneous unpacking support for unions

---

_@AlexWaygood_

## Summary

This PR adds precise heterogeneous unpacking support for unions.

For a tuple such as `tuple[int, str]`, we've long recognised that if you unpack this tuple, the first element will be an `int` and the second will be a `str`. But the same has not been true for `tuple[Literal[42], str] | tuple[Literal[56], str]` -- if a user unpacked this union of tuples, we would infer that both first and second elements were of type `Literal[42, 56] | str`. This PR fixes that: we now infer that the first element will be of type `Literal[42, 56]` and the second element will be of type `str`.

This doesn't add much complexity to our iteration logic, fixes a number of false positives in the ecosystem, and (surprisingly!) leads to a [nice performance boost](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuplespec-union) on the colour-science benchmark.

## Test Plan

Mdtests added


---

_Label `ty` added by @AlexWaygood on 2025-09-14 16:46_

---

_Comment by @github-actions[bot] on 2025-09-14 16:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Closed by @AlexWaygood on 2025-09-14 19:26_

---

_Reopened by @AlexWaygood on 2025-09-14 19:26_

---

_Comment by @github-actions[bot] on 2025-09-14 19:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:39:21: error[not-iterable] Object of type `AnyStr@update` may not be iterable
- scrapy/utils/datatypes.py:91:66: error[not-iterable] Object of type `AnyStr@update` may not be iterable
- Found 1067 diagnostics
+ Found 1065 diagnostics

vision (https://github.com/pytorch/vision)
- references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type | Unknown, ...]` is not assignable to `list[type] | None`
+ references/classification/utils.py:420:5: error[invalid-assignment] Object of type `tuple[type, ...] | tuple[Unknown, ...]` is not assignable to `list[type] | None`

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_groupby.py:3059:32: error[parameter-already-assigned] Multiple values provided for parameter `freq` of function `date_range`
- Found 1617 diagnostics
+ Found 1616 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:236:21: error[not-iterable] Object of type `ListOrTupleOrSetAny@UniqueItems` may not be iterable
- Found 69 diagnostics
+ Found 68 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/utils/tests/test_multiclass.py:410:27: warning[possibly-missing-attribute] Attribute `toarray` on type `(Unknown & SparseABC) | (list[Unknown | list[Unknown | int]] & SparseABC) | (list[Unknown | list[Unknown | str]] & SparseABC) | ... omitted 11 union elements` may be missing
+ sklearn/utils/tests/test_multiclass.py:410:27: warning[possibly-missing-attribute] Attribute `toarray` on type `(Unknown & SparseABC) | (list[Unknown | list[Unknown | int]] & SparseABC) | (_NotAnArray & SparseABC) | ... omitted 11 union elements` may be missing

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/util/test_assert_extension_array_equal.py:108:41: error[invalid-argument-type] Argument to function `assert_extension_array_equal` is incorrect: Expected `bool | Literal["equiv"]`, found `@Todo | SparseArray`
- pandas/tests/util/test_assert_extension_array_equal.py:108:41: error[invalid-argument-type] Argument to function `assert_extension_array_equal` is incorrect: Expected `str`, found `@Todo | SparseArray`
- Found 3389 diagnostics
+ Found 3387 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/lovelace/websocket.py:58:46: error[invalid-argument-type] Argument to bound method `send_error` is incorrect: Expected `dict[str, Any] | None`, found `str`
- Found 13756 diagnostics
+ Found 13755 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-11 12:31_

---

_Comment by @github-actions[bot] on 2025-10-11 12:36_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 6 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| **Total** | **0** | **7** | **0** |

**[Full report with detailed diff](https://alex-tuplespec-union.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-tuplespec-union.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-11 12:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuplespec-union)

### Merging #20377 will **improve performances by 6.26%**

<sub>Comparing <code>alex/tuplespec-union</code> (8dde577) with <code>main</code> (4b7f184)</sub>



### Summary

`⚡ 1` improvement  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuplespec-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime) | 11.2 s | 10.5 s | +6.26% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuplespec-union?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_@AlexWaygood reviewed on 2025-10-11 13:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5534 on 2025-10-11 13:48_

other than the `Type::Union` branch, this is exactly the same code that used to exist in the `match` statement slightly lower down. It's just been extracted into a standalone function (so that it can be called recursively), and moved higher up to satisfy Clippy.

---

_Marked ready for review by @AlexWaygood on 2025-10-11 14:03_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-11 14:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-11 14:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-11 14:03_

---

_Converted to draft by @AlexWaygood on 2025-10-13 10:20_

---

_Marked ready for review by @AlexWaygood on 2025-10-13 10:20_

---

_Comment by @dcreager on 2025-10-14 00:51_

We've supported unpacking of unions like this [in assignments](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/resources/mdtest/unpacking.md#L762), but it looks like this implementation might be more general. Can we replace the older union-related code in `types/unpacker.rs` with calls to this new logic?

---

_Review request for @sharkdp removed by @sharkdp on 2025-10-14 07:15_

---

_Comment by @AlexWaygood on 2025-10-14 11:42_

> We've supported unpacking of unions like this [in assignments](https://github.com/astral-sh/ruff/blob/5e08e5451df62398caf16034ae50621e46688641/crates/ty_python_semantic/resources/mdtest/unpacking.md#L762), but it looks like this implementation might be more general. Can we replace the older union-related code in `types/unpacker.rs` with calls to this new logic?

That's a great question. I've just spent a while looking at this (probably longer than I should have!), and my answer is... I don't think so, unfortunately. Obtaining a different tuple spec for each union member, like `unpacker.rs` is doing, is still preferable to calling `try_iterate()` directly on a union type -- it's still less lossy, yields better fallback results in the case of errors, and gives better error messages.

I experimented with having `Type::try_iterate()` return `Result<IterationOutcome<'db>, IterationError<'db>>`, where `IterationOutcome` is an abstraction that looks like this:

```rs
struct IterationOutcome<'db>(smallvec::SmallVec<[Cow<'db, TupleSpec<'db>>; 1]>);
```

but even that is too lossy a representation for `unpacker.rs`. `unpacker.rs` really needs to know whether each individual union element is iterable, and what kind of diagnostic it emits if it's not. So for `unpacker.rs` to be able to use `try_iterate` without manually mapping over the union elements, `try_iterate` would need to return `IterationOutcome<'db>`, where `IterationOutcome` looks like

```rs
struct IterationOutcome<'db>(
    smallvec::SmallVec<[Result<Cow<'db, TupleSpec<'db>>, IterationError<'db>>; 1]>
);
```

At which point the abstraction becomes so complicated, that I think it's probably not worth it anymore.

So the question is: what does this PR actually get us?

Firstly it gets us much better call-binding for calls with variadic parameters: unlike in `unpacker.rs`, we do not map over the union elements manually in `call/bind.rs`. Maybe we _should_ map over the union elements there, like in `unpacker.rs`, but it looks awkward to pull off: https://github.com/astral-sh/ruff/blob/ac2c5303775bcf52d40eea406939a6961774e6af/crates/ty_python_semantic/src/types/call/bind.rs#L2283-L2291

Secondly, it gets us much more precise inference even if all you want is the _homogeneous_ element type. For example:

```py
from typing import Literal

def f(x: Literal["abc", "def"]):
    for item in x:
        # main: LiteralString
        # This PR: Literal["a", "b", "c", "d", "e", "f"]
        reveal_type(item)
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/call/function.md`:735 on 2025-10-14 19:39_

bit of a nit: Using `"a"`, etc as the values in `my_args` and `my_other_args` made me assume at first that you would be exercising `**` unpacking, where it's important that some of the values line up with parameter names. Maybe use integer literals instead of string literals to eliminate that confusion?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1116 on 2025-10-14 19:44_

Not blocking for this PR, but this pattern seems to come up enough to deserve a helper method on `Type` — a `map_over_union` that applies the function to each union element if it's a union, or to the individual type if not.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:1586 on 2025-10-14 19:45_

nit: Can you throw a `copied` onto each iterator so that this can return `Iterator<Item = Type>`? I find it good karma to not make the caller have to worry about that

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:1626 on 2025-10-14 19:47_

To be fair, such a branch wouldn't be _that_ much longer than the explanation for why it's not present... 😄  But I don't disagree with your argument, and am fine with leaving it out.

---

_@dcreager approved on 2025-10-14 19:51_

> That's a great question. I've just spent a while looking at this (probably longer than I should have!), and my answer is... I don't think so, unfortunately.

Thanks for looking at that! No argument with your findings. Can you summarize them in the code? Possibly in `unpacker.rs`, to explain why you can't use the union-handling logic that we use for argument splatting

---

_@AlexWaygood reviewed on 2025-10-14 20:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1626 on 2025-10-14 20:09_

Yeah, I had it originally, but then couldn't figure out a test for it that actually reflected something that might come up in real-world code, and the virtue of a comment is it doesn't need tests 😆 so I ripped it out

---

_@AlexWaygood reviewed on 2025-10-15 18:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:1586 on 2025-10-15 18:02_

Hmm, I feel like here it's more ergonomic to have it return `Iterator<Item = &Type<'db>>`, because then it matches the signature of `TupleSpec::all_elements` (and, indeed, the two other `all_elements()` methods in this module!):

https://github.com/astral-sh/ruff/blob/6f468ae8e33bf3ad1d342d29c049fb620c701ca0/crates/ty_python_semantic/src/types/tuple.rs#L343-L345

it seems like strictly more code for not much gain here, if I make it return `Iterator<Item = Type<'db>>` rather than `Iterator<Item = &Type<'db>>`?

```diff
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index dcb3df675d..cc39e696f8 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1583,7 +1583,7 @@ impl<'db> TupleSpecBuilder<'db> {
         }
     }
 
-    fn all_elements(&self) -> impl Iterator<Item = &Type<'db>> {
+    fn all_elements(&self) -> impl Iterator<Item = Type<'db>> {
         match self {
             TupleSpecBuilder::Fixed(elements) => Either::Left(elements.iter()),
             TupleSpecBuilder::Variable {
@@ -1592,6 +1592,7 @@ impl<'db> TupleSpecBuilder<'db> {
                 suffix,
             } => Either::Right(prefix.iter().chain(std::iter::once(variable)).chain(suffix)),
         }
+        .copied()
     }
 
     /// Return a new tuple-spec builder that reflects the union of this tuple and another tuple.
@@ -1625,8 +1626,10 @@ impl<'db> TupleSpecBuilder<'db> {
             // would actually lead to more precise inference, so it's probably not worth the
             // complexity.
             _ => {
-                let unioned =
-                    UnionType::from_elements(db, self.all_elements().chain(other.all_elements()));
+                let unioned = UnionType::from_elements(
+                    db,
+                    self.all_elements().chain(other.all_elements().copied()),
+                );
                 TupleSpecBuilder::Variable {
                     prefix: vec![],
                     variable: unioned,
```

---

_Merged by @AlexWaygood on 2025-10-15 18:30_

---

_Closed by @AlexWaygood on 2025-10-15 18:30_

---

_Branch deleted on 2025-10-15 18:30_

---
