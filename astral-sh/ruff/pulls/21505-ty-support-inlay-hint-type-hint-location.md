```yaml
number: 21505
title: "[ty] Support inlay hint type hint location"
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
base: main
head: inlay-hint-type-hint-location
created_at: 2025-11-17T21:21:21Z
updated_at: 2025-12-27T00:21:05Z
url: https://github.com/astral-sh/ruff/pull/21505
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Support inlay hint type hint location

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/1078.

[Screencast_20251117_212227.webm](https://github.com/user-attachments/assets/eb1249fe-83ad-4d2a-aed8-2ab33eb0fbcd).

To find valid identifiers in the display string we do the following.
1. iterate over each char
2. start with max length string (current char to end of string)
3. reduce string length until valid identifier is found
4. If no valid identifier is found, we append the character to the label parts.
5. if we do find one we check all global symbols (this should be sped up) to find the symbol we want.
6. Add inlay label part and navigation target.


## Test Plan

Our current snapshots for inlay hint locations are very hard to understand and frankly arent useful at all.

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 21:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 21:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-17 21:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MatthewMckee4 on 2025-11-17 21:41_

There are a lot of false positives (random targets) here.

- In "Todo" annotations
- In literal string annotations.

I think this is a good test but probably not a good solution. 

Interested to see people's thoughts.

---

_Marked ready for review by @MatthewMckee4 on 2025-11-17 21:42_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-11-17 21:42_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-11-17 21:42_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-11-17 21:42_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-11-17 21:42_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-11-17 21:42_

---

_Renamed from "Support inlay hint type hint location" to "[ty] Support inlay hint type hint location" by @amyreese on 2025-11-17 22:14_

---

_Label `server` added by @MichaReiser on 2025-11-18 08:13_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-18 08:13_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-18 08:13_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-18 08:13_

---

_Comment by @MichaReiser on 2025-11-18 08:27_

Thanks to work on this. 

It feels wrong to me to render the type to a string, and then trying to reparse the information. I think we should either:

* Accept the fact that the LSP needs more flexibility and use a separate rendering pipeline all together
* Use a visitor approach where we don't write to a `std::fmt::Formatter` but to our own formatter instead, which is a `trait` with `visit_type(&mut self, ty)`, `write_str` etc. methods. I'd have to play with the problem myself to get a good sense for how an ergonomic API for this would look like.

We do have a similar problem for showing go to targets on hover. It's slightly different than inlay but we would want to collect all types, resolve their definitions, and render the definitions in a separate list. That's why I think a visitor approach where one implementation writes to a normal formatter, another that creates the inlay hint parts, and a third that writes to a normal formatter and collects the go to definition targets seems best?

But curious to hear what @Gankra thinks. She worked on this code more than I

---

_Comment by @AlexWaygood on 2025-11-18 08:33_

> * Use a visitor approach where we don't write to a `std::fmt::Formatter` but to our own formatter instead, which is a `trait` with `visit_type(&mut self, ty)`, `write_str` etc. methods. I'd have to play with the problem myself to get a good sense for how an ergonomic API for this would look like.

For reference, we have a `TypeVisitor` trait at https://github.com/astral-sh/ruff/blob/d5a95ec8246aae0656c3be6b5b23741e84fa8a86/crates/ty_python_semantic/src/types/visitor.rs#L32, but I'm not totally sure from your description whether it would work for your purposes here 

---

_Comment by @MichaReiser on 2025-11-18 08:46_

I think we would want something more specific to rendering types. I very much doubt that we need separate visitor methods for each type. We're more interested in getting "hook-in-points" to know when we enter a new type and when we exit it, as well as having a way to write the label for the current type.

---

_Comment by @MatthewMckee4 on 2025-11-18 11:07_

> It feels wrong to me to render the type to a string

I agree

It also feels wrong to duplicate display logic from `display.rs`.

Could we make changes in `display.rs`? Possibly making some intermediate struct that collects all the parts of the string, importantly parts that have a definition (or just `TextRange`) that we can goto. Then implement `Display` for that.

Meaning we can get all the information we need from the intermediate struct.

---

_Label `ty` added by @AlexWaygood on 2025-11-18 11:09_

---

_Comment by @MichaReiser on 2025-11-18 12:15_

> Could we make changes in display.rs? Possibly making some intermediate struct that collects all the parts of the string, importantly parts that have a definition (or just TextRange) that we can goto. Then implement Display for that.

I think this would work but has two downsides:

* We collect a lot of unnecessary information for the common case where we just want to render a type to a string
* It's not enough for go to in hover (https://github.com/astral-sh/ty/issues/1575), requiring another refactor.

The approach I would try is to define a new trait:

```rust
pub trait DisplayTypeFormatter<'db> {
	fn enter_type(&mut self, ty: Type<'db>) {}
	fn exit_type(&mut self) {}
	fn write_str(&mut self, part: &str);
}
```

I think we can even implement `std::fmt::Write` for `DisplayVisitor` so that we can keep using `write!(f, "text{interpolation}");` by either implementing it for `&dyn DisplayTypeFormatter` or generic over `T`

```rust
impl std::fmt::Write for dyn DisplayTypeFormatter<'_> {
	fn write_str(&mut self, text: &str) -> std::fmt::Result<()> {
		DisplayFormatter::write_str(self, text);
		Ok(())
	}
}
```

The implementation to formatting the type to a string is:

```rust
struct DisplayToString(String);

impl DisplayTypeFormatter<'_> for DisplayToString {
	fn write_str(&mut self, part: &str) {
		self.0.push_str(part);
	}
}
```

Extracting the inlay is a bit tricky. It will require maintaining a stack of inlay parts where you push to the stack when entering a new type and popping from the stack when exiting (the stack is your scratch pad and you move them to a finalized state once finished.


The hover implementation wraps a `DisplayTypeFormatter` and also keeps a `HashSet` of all seen `Definition`.

Note: I haven't tried any of this, so this might not work or require more methods or different signatures but I think this is worth exploring.


---

_Comment by @Gankra on 2025-11-18 19:27_

Oh how serendipitous, I was just thinking about implementing this. We already have the visitor pattern in our display code, it's used by signature_help. I would absolutely implement this feature by expanding upon this design:

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ty_python_semantic/src/types/display.rs#L1099-L1109

https://github.com/astral-sh/ruff/blob/192c37d5401786b3043aafbfb657b4f5e7df8ac8/crates/ty_python_semantic/src/types/display.rs#L1155-L1160

---

_Comment by @MatthewMckee4 on 2025-11-19 11:00_

Cool, I'll close this for now and let you do your thing, I'm happy to link it up to the inlay hints if you don't get to that.

I will also think about a better way to snapshot the targets.

---

_Closed by @MatthewMckee4 on 2025-11-19 11:00_

---

_Comment by @Gankra on 2025-11-20 14:41_

@MatthewMckee4 done in #21533, thanks for getting the ball rolling!

---

_Branch deleted on 2025-12-27 00:21_

---
