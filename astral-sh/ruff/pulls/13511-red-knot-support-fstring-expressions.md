```yaml
number: 13511
title: "[red-knot] support fstring expressions"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/support-fstring-expressions
created_at: 2024-09-25T11:46:04Z
updated_at: 2024-09-27T18:20:18Z
url: https://github.com/astral-sh/ruff/pull/13511
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] support fstring expressions

---

_@Slyces_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement inference for `f-string`, contributes to astral-sh/ty#244.

### First Implementation

When looking at the way `mypy` handles things, I noticed the following:
- No variables (e.g. `f"hello"`) ⇒ `LiteralString`
- Any variable (e.g. `f"number {1}"`) ⇒ `str`

My first commit (1ba5d0f13fdf70ed8b2b1a41433b32fc9085add2) implements exactly this logic, except that we deal with string literals just like `infer_string_literal_expression` (if below `MAX_STRING_LITERAL_SIZE`, show `Literal["exact string"]`)

### Second Implementation

My second commit (90326ce9af5549af7b4efae89cd074ddf68ada14) pushes things a bit further to handle cases where the expression within the `f-string` are all literal values (string representation known at static time).

Here's an example of when this could happen in code:
```python
BASE_URL = "https://httpbin.org"
VERSION = "v1"
endpoint = f"{BASE_URL}/{VERSION}/post"  # Literal["https://httpbin.org/v1/post"]
```
As this can be sightly more costly (additional allocations), I don't know if we want this feature.

## Test Plan

- Added a test `fstring_expression` covering all cases I can think of


---

_Review requested from @carljm by @Slyces on 2024-09-25 11:46_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-25 11:46_

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-25 11:46_

---

_Renamed from "Feat/support fstring expressions" to "support fstring expressions" by @Slyces on 2024-09-25 11:46_

---

_@MichaReiser reviewed on 2024-09-25 11:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 11:58_

Using the display representation to get the value of the type is dangerous. We could accidentally change the inferred literal value by changing how those types are displayed.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1660 on 2024-09-25 11:59_

Could we, instead of using a `literals` `Vec` and an `expr_arena` instead use a `String` and concatenated the parts in-place? It also allows you to bail out early as soon as the string exceeds the max string literal size instead of iterating to the end.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-09-25 12:00_

Is this change intentional?

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 12:00_

That's fair - would you re-code the representation here then or just not provide this feature for bool/int?

---

_@Slyces reviewed on 2024-09-25 12:00_

---

_@MichaReiser reviewed on 2024-09-25 12:00_

Oh wow nice and thanks for explaining how mypy handles this.

---

_Label `red-knot` added by @MichaReiser on 2024-09-25 12:00_

---

_@Slyces reviewed on 2024-09-25 12:03_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-09-25 12:03_

I think - as far as I understand, `infer_fstring_expression` does not mutate anything in self, so calling it without using the returned value does nothing, doesn't it?
As far as the return we have (`Type::Unknown`), it should still be this as ` f-strings` are invalid in annotation context (to my knowledge)

---

_@Slyces reviewed on 2024-09-25 12:09_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1660 on 2024-09-25 12:09_

I think my intention was to concatenate last minute so very long `f-string` with a variable interpolation fairly late are not allocated (we keep everything as `Box<str>` as long as possible).

Example:
`f"this is a very long sequence of characters [...] then I interpolate {x}"`

There is the problem of allocating `Int` / `Bool` repr, but they realistically should never be too big (unless a pathological case is crafted on purpose).

I'll try out the `String` version for sure though, as I didn't think about it when implementing.

---

_Comment by @github-actions[bot] on 2024-09-25 12:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Slyces reviewed on 2024-09-25 12:28_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1660 on 2024-09-25 12:28_

Implemented in 3ee3fcbc3bacc40df0bf13dbdefdcc57c7655757 if you want to compare versions

---

_@MichaReiser reviewed on 2024-09-25 12:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-09-25 12:28_

Not entirely sure. We might still need to call it to collect any diagnostics. @carljm ?

---

_@MichaReiser reviewed on 2024-09-25 13:05_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 13:05_

I don't mind the feature, although it probably goes a bit out of scope for this PR :) 

I would re-code it or even add a dedicated method on `Type` because it is important that the value returned from this method exactly matches the runtime semantics.

---

_Renamed from "support fstring expressions" to "[red-knot] support fstring expressions" by @AlexWaygood on 2024-09-25 14:44_

---

_@Slyces reviewed on 2024-09-25 16:08_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 16:08_

Just to be clear, what do you mean by "the runtime semantics"? Is there a source I can refer to to know what's the "correct" display for a given int/bool?

---

_@MichaReiser reviewed on 2024-09-25 16:42_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 16:42_

You can evaluate the expression in Python. The code here isn't about how we display type's. It's what the value of the f-string is at runtime, which requires evaluating the expressions the same as Python would at runtime. 

---

_@AlexWaygood reviewed on 2024-09-25 16:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 16:52_

Yes, we could have a `Type::repr()` method that returns `Option<String>`, that attempts to exactly emulate the result of callling `repr()` on an instance of the type at runtime in Python (or returns `None` if we can't statically determine what the repr would be). This might be different from the `representation` method in some cases, as the `representation` method is just an internal implementation detail of how we implement `Display` for various types. Using `representation` works fine for now, but could easily break in the future.

---

_@AlexWaygood reviewed on 2024-09-25 16:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 16:58_

Although for unions, for example, we might have multiple possible `repr()`s of a value, so maybe `Option<Vec<String>>` would be a better return type for `repr`. E.g. calling repr on `Type::Union([StringLiteral("foo"), StringLiteral("bar")])` might return `Some([String("foo"), String("bar")])` -- we know the repr of an object of that type will be _one_ of those two strings

---

_@MichaReiser reviewed on 2024-09-25 17:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:01_

I'm not sure if I would go as far as returning a `Vec`. It's unclear how and where we would use that. But right, `repr` is the behavior we need :)

---

_@MichaReiser reviewed on 2024-09-25 17:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1660 on 2024-09-25 17:02_

> Implemented in [3ee3fcb](https://github.com/astral-sh/ruff/commit/3ee3fcbc3bacc40df0bf13dbdefdcc57c7655757) if you want to compare versions

Thanks. I do like that better

---

_@AlexWaygood reviewed on 2024-09-25 17:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:05_

Consider this code:

```py
from typing import Literal

def foo(x: Literal["foo", "bar"]):
    y = f"{x}{x}"
```

The repr of `x` will be either `"foo"` or `"bar"`. If `Type::repr` returned a vec of possible reprs, we could reasonably infer that the value of `y` will be either `"foofoo"` or `"barbar"`, meaning the type of y will be `Literal["foofoo", "barbar"]`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:07_

(@JelleZijlstra who is sitting next to me at the core dev sprint informs me that his type checker, pyanalyze, already has this feature 😄)

---

_@AlexWaygood reviewed on 2024-09-25 17:07_

---

_@MichaReiser reviewed on 2024-09-25 17:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:08_

We could but I think it's going a bit too far. You very quickly end up with quadratic complexity and I'm not sure how useful the precise type is when refining. 

Like what are actual code patterns that this enables. Why does it only work with f strings and not format strings?

I do like the feature but I think we should keep it basic for now. 

---

_@MichaReiser reviewed on 2024-09-25 17:11_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:11_

Does he find it to be a useful feature?

---

_@AlexWaygood reviewed on 2024-09-25 17:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:13_

I'm fine returning `None` for unions for now. You're right that it is quadratic in behaviour, and Jelle doesn't think it's a crucial feature 😄

---

_@JelleZijlstra reviewed on 2024-09-25 17:17_

---

_Review comment by @JelleZijlstra on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:17_

Yeah it's not that practically useful, I likely implemented it just because it was fairly simple to do and helps to infer more precise types. But I don't think inferring precise string literal types is likely to make the rest of type inference better.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1673 on 2024-09-25 17:26_

I think we do want `Type::repr`, and we should use it here. I think it's fine to either entirely leave that as a TODO in this PR, or it's also fine to add a simple `Type::repr` in this PR that handles a few simple types and leaves the rest as TODO. (Main advantage of the latter is that you already have tests in this PR for f-strings including ints, and it will be simpler to keep them than have to take them out and re-add them later.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1690 on 2024-09-25 17:28_

Can't we also break early as soon as we have `has_expression` true?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-09-25 17:32_

We maintain an invariant that all expressions in a scope are visited when inferring types in that scope. Since f-strings can have arbitrary sub-expressions, failing to call `infer_fstring_expression` here can mean that we fail to visit those sub-expressions or assign them a type (and potentially miss diagnostics in them.)

There will be some decisions to make at some point around how we want to handle invalid expression kinds in annotations -- should we still emit internal diagnostics for them, or just emit the "not valid in annotations" diagnostic, assign type unknown, and move on? I don't have a clear answer on that right now, but for this PR, for now, I think this change should be reverted so that we maintain our "visit all expressions" invariant.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3650 on 2024-09-25 17:33_

If you keep the bool repr support, add an interpolated boolean here as well

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3407 on 2024-09-25 17:35_

This doesn't look necessary to me; it doesn't test anything that isn't already tested above. I don't think tests necessarily need to demonstrate a realistic-looking use case. If you want to make the above test look more realistic, that's fine, but IMO it's not worth writing and inferring types in another file.

If we did have a reason to keep this, it should be a separate test method.

---

_@carljm reviewed on 2024-09-25 17:36_

This is great, thank you! I think there are still a couple things to address before landing.

---

_@Slyces reviewed on 2024-09-25 18:55_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2585 on 2024-09-25 18:55_

> We maintain an invariant that all expressions in a scope are visited when inferring types in that scope. 

That's very interesting to know. I think this applies to my PR as there is (in the latest version) a short-circuit logic to stop iterating over `f-string` parts (that could be arbitrarily long) once we know for sure the type to infer (either `str` instance, or `StringLiteral`).

Should I go back and ensure that we do call `infer_expression` even when we already figured out the return type?

---

_@Slyces reviewed on 2024-09-25 19:08_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1690 on 2024-09-25 19:08_

We can - I think I had this in a version and took it out by mistake. But I also think that according to one of your comments somewhere else breaking early implies (as of now) that we don't infer the type of some expressions.

---

_@Slyces reviewed on 2024-09-25 19:11_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:3407 on 2024-09-25 19:11_

I think I mainly had it to make sure my comment in the PR's body was accurate, and committed it by mistake. Good catch :)

---

_@carljm reviewed on 2024-09-25 19:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1690 on 2024-09-25 19:13_

Ah, good point! Yes, we should infer a type for all expressions, so we need to continue the loop. But we can set a `done` flag so we don't bother looking at the type for subsequent parts, or concatenating anything.

---

_Comment by @Slyces on 2024-09-25 21:57_

I implemented the methods `Type::repr` to use in `f-strings` inference.

While doing so, I figured out a couple things:
- String replacement in `f-strings` is actually using `str` (see [here](https://docs.python.org/3/reference/datamodel.html#object.__str__))
- The `str` method uses `repr` as a fallback
- There is a rabbit-hole of the `format` builtin (see [here](https://docs.python.org/3/reference/datamodel.html#object.__str__)) to deal with things like `f"total ${number:01}"` that are out of scope with this PR

---

_Comment by @AlexWaygood on 2024-09-25 22:00_

> String replacement in `f-strings` is actually using `str` (see [here](https://docs.python.org/3/reference/datamodel.html#object.__str__))

Ah! Very good point. I forgot about this -- `str()` is used unless `!r` is given as the format specifier, in which case `repr` is used :-)

---

_Comment by @JelleZijlstra on 2024-09-25 22:00_

> String replacement in f-strings is actually using str

It actually uses `__format__` (unless you use the `!s` modifier). However, for most objects, this is the same.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:679 on 2024-09-25 22:37_

Nit: calling `str` will involve `Type::call` on the `str` type, so it's clearer if here we only mention the `__str__` builtin, which is what this method really represents.
```suggestion
    /// Return the string representation of this type when converted to string as it would be
    /// provided by the `__str__` method. If that can't be determined, return `None`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:695 on 2024-09-25 22:38_

```suggestion
    /// Return the string representation of this type as it would be provided by the  `__repr__` method. If that can't be determined, return `None`.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:701 on 2024-09-25 22:39_

The repr of a string actually includes enclosing single quotes:

```
>>> s = "foo"
>>> repr(s)
"'foo'"
```

So we should add the single quotes here, and copy this implementation (without the single quotes) up into `Type::str`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:17 on 2024-09-25 22:39_

I think we can make this private again? It really shouldn't be used outside this module.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:696 on 2024-09-25 22:46_

I think that both this method and `Type::str` should instead return a `Type<'db>`, which in practice will always be either `Type::LiteralString` or a `Type::StringLiteral`. In some other cases, returning a custom enum allows more flexibility in the caller and sometimes more efficiency, but that's not the case here. Returning a `String` means allocating, and we don't even get to take advantage of type interning; we'll only ever have a single `StringLiteralType("True")` (because of Salsa interning), but we could end up allocating many different "True" Strings from this method as currently written. Also, it means that `Type::str` on a `StringLiteral` can just return the same type, not allocate a new owned String.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:687 on 2024-09-25 22:47_

This is not correct for `StringLiteral`, as mentioned below.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1669 on 2024-09-25 22:49_

This might be quite complex to do fully, and may not be a priority for quite some time, if ever. But I don't want us to give _incorrect_ results in the meantime. It seems like it should be quite easy to detect whether there is a format string at all, and if so, just fall back to `LiteralString` instead of a specific `StringLiteral` type. That seems worth doing now?

We should still have a TODO for handling `__format__` and format specifiers (at least for some types); I'd like that TODO to specifically mention `__format__`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3660 on 2024-09-25 22:58_

Let's also add a test with a known value with a format spec and test (for now) that we fallback to `LiteralString`.

---

_@carljm reviewed on 2024-09-25 22:58_

---

_@Slyces reviewed on 2024-09-26 07:01_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:679 on 2024-09-26 07:01_

I'll do the same for `__repr__`

---

_@Slyces reviewed on 2024-09-26 09:44_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1669 on 2024-09-26 09:44_

That's done - I realised however that there's more work than I expected there

- Conversion flags should be handled before calling format
- Format logic could be re-coded specifically for builtins
- From [this](https://docs.python.org/3/library/functions.html#format), we should keep track of non-empty format specifiers reaching the `object` implementation
> Changed in version 3.4: object().__format__(format_spec) raises [TypeError](https://docs.python.org/3/library/exceptions.html#TypeError) if format_spec is not an empty string.

---

_Review requested from @carljm by @Slyces on 2024-09-26 12:29_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-26 12:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:730 on 2024-09-26 17:13_

Sorry, I wasn't clear; I don't think there's any reason for this to return an `Option`. The runtime enforces that `__str__` methods must return a string (otherwise it fails with a `TypeError`), so we can trust this. That means the only possible return values here are `str`, `LiteralString`, or a `StringLiteral`. So in any "can't be determined" case, we can always correctly fall back to returning the `str` type; there is no truly "unknown" case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:727 on 2024-09-26 17:14_

```suggestion
    /// provided by the `__str__` method.
    ///
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:743 on 2024-09-26 17:15_

Same as above, I don't think we need to return an `Option` here; the "we don't know" fallback case can simply be `str`. That is, `builtins_symbol_ty(db, "str").to_instance(db)`

```suggestion
    /// method at runtime.
    pub fn repr(&self, db: &'db dyn Db) -> Type<'db> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1683 on 2024-09-26 17:22_

nit: why `!conversion.is_none()` instead of `conversion.is_some()`?

---

_@carljm reviewed on 2024-09-26 17:22_

This is very close, thanks for sticking with it! Just a couple small comments yet.

---

_@Slyces reviewed on 2024-09-26 18:33_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:730 on 2024-09-26 18:33_

Oh I didn't get that indeed, sorry! I'll fix it soon 🙂

---

_@Slyces reviewed on 2024-09-26 20:28_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1683 on 2024-09-26 20:28_

Because conversion is not an option but an enum with the `None` variant and the `Is` macro derived 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:743 on 2024-09-27 06:55_

We don't have to do this in this PR but we could consider adding a `Type::builtin_str` factory function that returns `builtins_symbol_ty(db, "str")` to avoid repeating it all over the place ;)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:756 on 2024-09-27 06:58_

This requires escaping: 


```
>>> repr("ab'cd")
'"ab\'cd"'
```

I think you can use https://doc.rust-lang.org/std/primitive.str.html#method.escape_default

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1709 on 2024-09-27 07:08_

Checking the string length only after appending to `concatenated` has the downside that the implementation copies over very long strings only to then decide that this was unnecessary

I recommend to eagerly check the string length before appending

```rust
struct Collector {
	concatenated: Option<String>,
	expression: bool,
}

impl Collector {
	fn new() -> Self {
		Self { concatenated: Some(String::new()), expression: false }
	}

	fn push_str(&mut self, literal: &str) {
		self.concatenated = self.concatenated.as_mut().and_then(|concatenated| {
			if concatenated.len().saturating_add(literal.len()) <= MAX_STRING_LITERAL_SIZE {
				concatenated.push_str(literal);
				Some(concatenated)
			} else {
				None
		})
	}

  fn add_expression(&mut self) {
		self.concatenated = None;
		self.expression = true;
	}

  fn ty(self, db: &dyn Db) -> Type {
		if self.expression {
			builtins_symbol_ty(self.db, "str").to_instance(db)
		} else if let Some(concatenated) = self.concatenated { 
			Type::StringLiteral(StringLiteralType::new(db, concatenated.into_boxed_str()))
		} else {
			Type::LiteralString
		}
	} 
}
```

---

_@MichaReiser approved on 2024-09-27 07:08_

---

_@Slyces reviewed on 2024-09-27 11:25_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:1709 on 2024-09-27 11:25_

Thank you for this suggestion!

---

_Review requested from @carljm by @Slyces on 2024-09-27 12:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:57 on 2024-09-27 16:52_

```suggestion
struct DisplayRepresentation<'db> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:740 on 2024-09-27 16:55_

```suggestion
            _ => Type::builtin_str(db).to_instance(db),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:763 on 2024-09-27 16:55_

```suggestion
            _ => Type::builtin_str(db).to_instance(db),
```

---

_@carljm approved on 2024-09-27 16:59_

---

_Merged by @carljm on 2024-09-27 17:29_

---

_Closed by @carljm on 2024-09-27 17:29_

---

_Branch deleted on 2024-09-27 18:20_

---
