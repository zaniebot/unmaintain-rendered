```yaml
number: 13113
title: "red-knot: infer string literal types"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-infer-string-literal
created_at: 2024-08-26T17:39:25Z
updated_at: 2024-08-27T23:45:01Z
url: https://github.com/astral-sh/ruff/pull/13113
synced_at: 2026-01-10T21:38:32Z
```

# red-knot: infer string literal types

---

_Pull request opened by @chriskrycho on 2024-08-26 17:39_

## Summary

Introduce a `StringLiteralType` with corresponding `Display` type and a relatively basic test that the resulting representation is as expected.

Note: we currently always allocate for `StringLiteral` types. This may end up being a perf issue later, at which point we may want to look at other ways of representing `value` here, i.e. with some kind of smarter string structure which can reuse types. That is most likely to show up with e.g. concatenation.

Contributes to astral-sh/ty#244.

## Test Plan

Added a test for individual strings with both single and double quotes as well as concatenated strings with both forms.


---

_Review requested from @carljm by @chriskrycho on 2024-08-26 17:39_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-26 17:39_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-26 17:39_

---

_@chriskrycho reviewed on 2024-08-26 17:41_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types.rs`:285 on 2024-08-26 17:41_

I expect this is probably something in the space of what we say for `BytesLiteral`?

---

_@chriskrycho reviewed on 2024-08-26 17:42_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2328 on 2024-08-26 17:42_

Should these have quotes in them? Or is this the expected form?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:56 on 2024-08-26 17:55_

I don't think we should need this for strings; they are already guaranteed to be valid UTF-8. We can just write the string directly.

(If you think there's a case where we need this, we should have a test that fails without it.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2328 on 2024-08-26 17:59_

These should have quotes; see the examples at https://typing.readthedocs.io/en/latest/spec/literal.html#literal

---

_@carljm requested changes on 2024-08-26 18:00_

Couple small changes, but generally looks great!!

---

_@carljm reviewed on 2024-08-26 18:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2328 on 2024-08-26 18:04_

(Quotes are necessary to disambiguate `Literal["1"]`, a string literal type, from `Literal[1]`, an int literal type.)

---

_@chriskrycho reviewed on 2024-08-26 18:10_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2328 on 2024-08-26 18:10_

That latter bit was my concern, so 👍🏼 – will push a fix for that shortly!

---

_@chriskrycho reviewed on 2024-08-26 18:10_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/display.rs`:56 on 2024-08-26 18:10_

Ah, very good.

---

_Comment by @github-actions[bot] on 2024-08-26 18:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-08-26 18:40_

Excellent!

---

_Merged by @carljm on 2024-08-26 18:42_

---

_Closed by @carljm on 2024-08-26 18:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:284 on 2024-08-26 18:42_

This is a somewhat annoying and pedantic nitpick from me, but: this comment isn't _strictly_ accurate. In between `Literal[<some string value>]` and `str`, there's another Python type: [`typing.LiteralString`](https://typing.readthedocs.io/en/latest/spec/literal.html#literalstring). Explaining exactly how `LiteralString` works would take quite a few words and isn't really relevant to this PR 😄 but for now I think I'd just use this as the TODO comment instead:

```suggestion
                // TODO defer to `typing.LiteralString`/`builtins.str`
                // methods from typeshed's stubs
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:390 on 2024-08-26 18:43_

Maybe we could use `Box<str>` here, to save a bit of space, since the data should always be immutable? Though it maybe wouldn't make much difference

---

_@AlexWaygood reviewed on 2024-08-26 18:46_

Nice! (Sorry for the post-merge review; I've been out for most of the day)

---

_@carljm reviewed on 2024-08-26 18:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:284 on 2024-08-26 18:58_

I guess the relevance to this specific TODO comment is that for many members of a string `Literal`, we can infer the return type as a `LiteralString` instead of `str`? This is true. It doesn't seem critical to me to reflect that detail in this TODO comment, though it certainly doesn't hurt. (The other nitpick on the TODO for both this and bytes is that for some methods we can probably safely infer an actual `Literal` type for the member, e.g. we can infer `Literal["foo"].upper()` to be `Literal["FOO"]` -- but I also didn't think we needed to mention that in the TODO.)

---

_@carljm reviewed on 2024-08-26 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:390 on 2024-08-26 19:00_

Great call; this is probably worth doing, considering it saves an entire usize of memory, which is eight ascii characters (on a 64-bit system); it could halve the memory used for a lot of small string literal types. (Not really, if you consider the Salsa interning overhead as well. But still, not insignificant if there are lots of small string literals.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:284 on 2024-08-26 19:02_

Right, yeah -- as I say, it's kind of a pedantic nitpick. But given the problems that mypy has had with implementing `LiteralString` (well, it still doesn't actually have an implementation, because it requires rewriting a lot of assumptions across the code), I'd prefer for us to keep `LiteralString` in mind from the very beginning when thinking about implementing `str` and `Literal[<some string value>]` types.

---

_@AlexWaygood reviewed on 2024-08-26 19:02_

---

_Branch deleted on 2024-08-26 19:16_

---

_@chriskrycho reviewed on 2024-08-26 23:06_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types.rs`:390 on 2024-08-26 23:06_

Ah! Yeah, I consistently forget that `Box<str>` is an option. 👍🏼

---

_@chriskrycho reviewed on 2024-08-26 23:09_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types.rs`:284 on 2024-08-26 23:09_

Will update in a follow-on PR tomorrow – and I am personally a *big* fan of pedantic nitpicks when it comes to stuff like this. 😅

---

_Label `red-knot` added by @carljm on 2024-08-27 23:45_

---
