```yaml
number: 13917
title: "[red-knot] Slice expression types & subscript expressions with slices"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/infer-slice-expressions
created_at: 2024-10-24T18:10:00Z
updated_at: 2024-10-29T20:33:37Z
url: https://github.com/astral-sh/ruff/pull/13917
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Slice expression types & subscript expressions with slices

---

_Pull request opened by @sharkdp on 2024-10-24 18:10_

## Summary

- Add a new `Type::SliceLiteral` variant
- Infer `SliceLiteral` types for slice expressions, such as `<int-literal>:<int-literal>:<int-literal>`.
- Infer "sliced" literal types for subscript expressions using slices, such as `<string-literal>[<slice-literal>]`.
- Infer types for expressions involving slices of tuples: `<tuple>[<slice-literal>]`.

closes #13853

## Eye candy

```py
t = (1, (), True, "a", None, b"b")

reveal_type(t[-2::-2])  # revealed: tuple[None, Literal[True], Literal[1]]
```

## Test Plan

- Unit tests for indexing/slicing utility functions
- Markdown-based tests for
  - Subscript expressions `tuple[slice]`
  - Subscript expressions `string_literal[slice]`
  - Subscript expressions `bytes_literal[slice]`

---

_Label `red-knot` added by @sharkdp on 2024-10-24 18:10_

---

_Comment by @github-actions[bot] on 2024-10-24 18:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-10-28 11:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:893 on 2024-10-28 11:45_

`slice` appears to have no custom `__bool__` logic; even empty slices and things like `slice(None, None)` are `True`.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1830 on 2024-10-28 11:52_

Another option I considered was to inline this within `Type`. This would increase the size of `Type`. Unless we think it's okay to restrict indices for literal slice types to `i32`. Which might be fine. It seems unlikely that someone has a >2 GiB literal string inside their Python source code *and still* cares about inferring a static type for something like `str_literal[4_294_967_297:]`.

---

_@sharkdp reviewed on 2024-10-28 11:52_

---

_@MichaReiser reviewed on 2024-10-28 11:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1830 on 2024-10-28 11:59_

I think using an `i32` is probably okay. Ruff/Red Knot also only supports files `len(source) = u32::MAX`

---

_@sharkdp reviewed on 2024-10-28 13:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1505 on 2024-10-28 13:08_

Do we have a policy for choosing these names?

---

_@sharkdp reviewed on 2024-10-28 13:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3215 on 2024-10-28 13:14_

This looks like a regression in terms of (1) code quality and (2) functionality, as we previously supported indices up to i64 size. However, this had some drawbacks on 32 bit platforms. If the (absolute value of the) `i64` index did not fit into `usize`, we would show an "index out of bounds error", even though we hadn't even checked the size of the input. It's also highly unlikely that someone would construct a tuple/literal-string/literal-bytes object > 2GiB in size and still care about subscript type inference on that expression. Restricting indices to 32bit makes the error handling much easier, as the index->usize conversion can not fail.

---

_@sharkdp reviewed on 2024-10-28 13:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3236 on 2024-10-28 13:15_

I would have preferred to explicitly match on `Err(StepSizeZero)` here, but clippy forces me to use `if let … else …`.

---

_@MichaReiser reviewed on 2024-10-28 13:22_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3236 on 2024-10-28 13:22_

hehe... that's one of the pedantic lint rules that I'm very keen on disabling if I can find the necessary support.

---

_@sharkdp reviewed on 2024-10-28 14:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3273 on 2024-10-28 14:05_

This additional allocation is unfortunate, but `.chars()` is not an `ExactSizeIterator` (Unicode…), and I currently don't see how we can implement the most general form of slicing (e.g. something like `string_literal[3:-5]`) on `DoubleEndedIterator` alone. I could avoid the allocation by computing the length upfront (`O(n)`), but I'm not sure if it's worth investing more time into this.

---

_@sharkdp reviewed on 2024-10-28 14:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:39 on 2024-10-28 14:33_

I didn't bother repeating all tests here, since that seems sufficiently covered by the tuple/string cases + the slicing unit tests.

---

_@sharkdp reviewed on 2024-10-28 14:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:29 on 2024-10-28 14:34_

These tests are not exhaustive in terms of checking slicing functionality, as we do that in the unit tests already. Let me know if you think otherwise.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3249 on 2024-10-28 14:40_

These `.try_from(…).unwrap()`s don't look great. `int as i32` causes clippy to raise a lint, as it can't detect that it's safe. Let me know if you have any ideas on how to improve this.

---

_@sharkdp reviewed on 2024-10-28 14:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:109 on 2024-10-28 14:46_

This code isn't hot so unlikely that it's worth it but it's unfortunate that we have to allocate a `String` just to write the literal
```suggestion
							f.push_str("slice[")?;
							if let Some(start) = slice.start(self.db) {
								write!(f, "Literal[{start}]")?;
							} else {
								f.push_str("None")?;
							}
							
							f.push_str(", ")?;
							
							if let Some(stop) = slice.stop(self.db) {
								write!(f, "Literal[{stop}]")?;
							} else {
								f.push_str("None")?;
							}
							
							if let Some(step) = slice.step(self.db) {
								write!(f, ", Literal[{step}])?;
							}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1505 on 2024-10-28 14:47_

Not yet. But it's probably going to follow Ruff's naming schema ([internal document](https://www.notion.so/astral-sh/Rule-Guidance-1324991853284188ae3bc15f08c07afa#482c66b35f6a4747a635d0066ff9dcad))

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3249 on 2024-10-28 14:49_

I wouldn't mind using `#[allow(lint, reason="Checked in branch arm")]` or changing it to use `expect` and mention the reason there. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3273 on 2024-10-28 14:49_

I think this is fine, considering that slices into strings should be rare. We can optimize if this shows up in profiles.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/util/subscript.rs`:68 on 2024-10-28 14:51_

```suggestion
            Nth::FromEnd(nth_rev) => self.nth_back(nth_rev).ok_or(OutOfBoundsError),
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/util/subscript.rs`:89 on 2024-10-28 14:52_

I haven't looked into the usages of the `PySlice` type but would it make sense to implement it for `&[T]` instead of implementing it on iterators? Or are there cases where we only have an iterator?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/util/subscript.rs`:126 on 2024-10-28 14:53_

I think you could use `Itertools::iter::Either` to return one or the other iterator type without allocating

---

_@MichaReiser reviewed on 2024-10-28 14:54_

---

_@sharkdp reviewed on 2024-10-28 17:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1830 on 2024-10-28 17:54_

Switched to `i32` now as this also solves some problems on 32 bit platforms (see other comment). It's still `salsa::interned` for now, as the current size (2 × `Option<i32>` + 1 × `Option<NonZero<i32>>` = 20 byte) is still larger than `Type` at the moment (16 byte). If we absolutely want to avoid having this `interned`, we could certainly also go lower with the index range or do some more questionable hacks like using `i32::MIN` as a sentinel value instead of using `Option`, …

---

_@sharkdp reviewed on 2024-10-28 18:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1505 on 2024-10-28 18:24_

Ok, I tried something else for now.

---

_@sharkdp reviewed on 2024-10-28 18:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:44 on 2024-10-28 18:24_

This is a weird edge case that pyright and mypy do not detect (mypy [crashes](https://mypy-play.net/?mypy=master&python=3.13&gist=2e56de8c5fc30cf5e37fe15348c341bf)). I'm not sure if it's worth having a separate diagnostic for, but it was easily to implement. We could also ignore it and simplify infer a non-literal type (`slice`) for something like `::0`.

---

_@sharkdp reviewed on 2024-10-28 18:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/util/subscript.rs`:89 on 2024-10-28 18:52_

Yes, thanks. We only use it on slices if we are okay with `collect`ing the chars in the `LiteralString` case.

---

_@sharkdp reviewed on 2024-10-28 18:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/util/subscript.rs`:126 on 2024-10-28 18:53_

Oh, cool — I didn't know about `itertools::Either`! Needed to adapt the zero-case a bit to also fit into the `Either::Left` side, but that's okay.

---

_Marked ready for review by @sharkdp on 2024-10-28 19:55_

---

_Review requested from @carljm by @sharkdp on 2024-10-28 19:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-28 19:55_

---

_@sharkdp reviewed on 2024-10-28 20:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2024-10-28 20:35_

In a previous version of this PR, I deferred reporting this error to the usage site where we try to slice a string/bytes-literal or a tuple. Raising the diagnostic here is too early. The slice might be used on a type with a custom `__getitem__` impl that makes use of `0` as a step size somehow.

I'll fix this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:44 on 2024-10-29 02:36_

Seems useful to detect it when we can, but I agree with your comment below that we should mirror the runtime in when its an error, and when it isn't (you can define a custom type that takes a zero-step slice and doesn't crash). Which I think means (considering "define a custom type" also includes subclasses of commonly sliceable builtin types) we would only actually issue an error when slicing a literal type. Which is the case here, so these assertions look good.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:29 on 2024-10-29 02:38_

No, that seems fine!

---

_@carljm approved on 2024-10-29 02:53_

Looks good to me, modulo the already-commented step-zero error change!

---

_Merged by @sharkdp on 2024-10-29 09:17_

---

_Closed by @sharkdp on 2024-10-29 09:17_

---

_Branch deleted on 2024-10-29 09:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3423 on 2024-10-29 16:10_

I guess you could avoid the `.expect()` call here by doing something like:

```suggestion
            Some(Type::IntLiteral(n)) => match i32::try_from(n) {
                Ok(i) => SliceArg::Arg(Some(i)),
                Err(_) => SliceArg::Unsupported
            }
```

And probably similar elsewhere. But it's probably not very important; the way you've done it is obviously safe

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/util/subscript.rs`:21 on 2024-10-29 16:26_

nit: I probably would have written this as

```suggestion
    usize::try_from(index)
        .expect("Should only ever pass a positive integer to `from_nonnegative_i32`")
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3233 on 2024-10-29 16:31_

nit: we could probably avoid the `.as_ref()` call here and elsewhere if we implemented `PySlice` for `&Box<[T]>` as well as `&[T]`

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3271 on 2024-10-29 16:40_

We seem to do this several times -- is it worth adding an `as_tuple` method to `SliceLiteralType`?

```rs
impl<'db> SliceLiteralType<'db> {
    fn as_tuple(&self, db: &dyn Db) -> (Option<i32>, Option<i32>, Option<i32>) {
        (self.start(db), self.stop(db), self.step(db))
    }
}
```

And then this could be just

```suggestion
                let (start, stop, step) = slice_ty.as_tuple(self.db)
```

---

_@AlexWaygood reviewed on 2024-10-29 17:16_

Nice, this is great! Some post-merge review nitpicks below, but nothing major.

One other more significant thing to note is that it looks like we don't catch this error currently, and ideally at some point we will:

```pycon
>>> "foo"["bar":"baz"]
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    "foo"["bar":"baz"]
    ~~~~~^^^^^^^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method
```

It seems we just infer `"foo["bar":"baz"]` as being `@Todo`: this is because we infer `"bar":"baz"` (accurately) as being a `builtins.slice` instance, which means we fallback to generic "call `str.__getitem__` with `slice` passed in" logic, and currently we get `@Todo` as the result whenever we try to call `str.__getitem__` because it's overloaded and we don't understand overloads yet. The problem here is that even when we _do_ understand overloads we won't catch this error with the logic as we currently have it; we'll just start inferring `str` as the result.

For comparison, [mypy _does_ catch this error](https://mypy-play.net/?mypy=latest&python=3.12&gist=5c7f1f2b64ad30f656fd2e6382ade8e0), but it also incorrectly flags code like this, which should pass without error:

```pycon
>>> class Spam:
...     def __getitem__(self, item: slice | int) -> int:
...         return 42
...         
>>> Spam()["foo":"bar"]
42
```

I think this problem will naturally solve itself once we:
1. Add better annotations to `str.__getitem__` in typeshed (it should probably accept slice[SupportsIndex | None, SupportsIndex | None, SupportsIndex | None]` now that `slice` is generic, rather than just `slice`)
2. Support generic types in red-knot.

But it might be worth adding a TODO comment somewhere mentioning that this is something we need to address

---

_@sharkdp reviewed on 2024-10-29 19:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3423 on 2024-10-29 19:30_

> I guess you could avoid the `.expect()` call here by doing something like:

Yes. I added the other three uses of this before. And there I really need/want it as a pattern guard, because I want control flow to fall through if `i32::try_from` fails. Because I don't want to repeat the whole logic for the "else" clause.

But right here, your solution is definitely better. Changed in #13982 

---

_@sharkdp reviewed on 2024-10-29 19:30_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/util/subscript.rs`:21 on 2024-10-29 19:30_

Changed in https://github.com/astral-sh/ruff/pull/13982

---

_@sharkdp reviewed on 2024-10-29 19:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3233 on 2024-10-29 19:32_

Maybe you have a nice solution for this, but I quickly tried and everything I got required changes to the trait structure or other larger-scale code changes. I left those two `.as_ref()`s for now.

---

_@sharkdp reviewed on 2024-10-29 19:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3271 on 2024-10-29 19:32_

I like it, thanks — changed in https://github.com/astral-sh/ruff/pull/13982

---

_Comment by @sharkdp on 2024-10-29 19:33_

> It seems we just infer `"foo["bar":"baz"]` as being `@Todo`

Yes

> this is because we infer `"bar":"baz"` (accurately) as being a `builtins.slice` instance, which means we fallback to generic "call `str.__getitem__` with `slice` passed in" logic, and currently we get `@Todo` as the result whenever we try to call `str.__getitem__` because it's overloaded and we don't understand overloads yet.

Exactly.

> The problem here is that even when we _do_ understand overloads we won't catch this error with the logic as we currently have it; we'll just start inferring `str` as the result.

:+1: Added a TODO comment in #13982.

---

_@AlexWaygood reviewed on 2024-10-29 20:33_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3233 on 2024-10-29 20:33_

https://github.com/astral-sh/ruff/pull/13983 :-)

---
