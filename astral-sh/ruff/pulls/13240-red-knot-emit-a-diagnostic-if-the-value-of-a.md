```yaml
number: 13240
title: "[red-knot] Emit a diagnostic if the value of a starred expression or a `yield from` expression is not iterable"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-iterables
created_at: 2024-09-04T12:05:17Z
updated_at: 2024-09-04T15:19:50Z
url: https://github.com/astral-sh/ruff/pull/13240
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Emit a diagnostic if the value of a starred expression or a `yield from` expression is not iterable

---

_@AlexWaygood_

## Summary

This PR builds on #13195. It ensures that we emit the `not-iterable` diagnostic on expressions such as the following:

```py
class NotIterable: pass

def foo():
    [*NotIterable()]
    yield from NotIterable()
```

The `Type::iterate()` method is refactored slightly so that it's easier for callers of the method to all emit the same diagnostic: it now accepts a `diagnostic_fn` callback that emits a diagnostic if the type is not found to be an iterable type.

I deliberately haven't tackled comprehensions in this PR, for two reasons:
1. There's some more complications there
2. I think @dhruvmanila has been working a lot on comprehensions/there are several TODOs in the code that are assigned to him!

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-04 12:05_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-04 12:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-04 12:05_

---

_Comment by @codspeed-hq[bot] on 2024-09-04 12:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-iterables)

### Merging #13240 will **not alter performance**

<sub>Comparing <code>alex/redknot-iterables</code> (53cc0a2) with <code>main</code> (46a4573)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-09-04 12:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:405 on 2024-09-04 12:21_

I prefer to avoid passing callback functions whenever possible. It tends to lead to more verbose code and monomorphization. 

I wonder if we could use an approach similar to BiomeJS [`ParsedSyntax`](https://github.com/MichaReiser/biome/blob/98deae6e2409c1f9904356f6a6106e25cf3a89d6/crates/biome_parser/src/parsed_syntax.rs#L31) which is similar to an `Option` but with domain-specific helper methods. 

For example, we could define

```rust
enum ResolvedType<'db> {
	Known(Type<'db>),
	Unknown
}

impl<'db> ResolvedType<'db> {
	pub fn unwrap_with_diagnostic(self, builder: &mut TypeInferenceBuilder, node: AnyNodeRef, rule: &str, message: std::fmt::Arguments) -> Type<'db> {
		match self {
			Known(ty) => ty,
			Unknown(ty) => {
				builder.add_diagnostic(node, rule, message);
				Type::Unknown
			}
 	}
  }
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:405 on 2024-09-04 12:24_

An alternative is that `unwrap_with_diagnostic` accepts an error builder similar to biomejs. This is fine because the `unwrap_with_diagnostic` function contains very little code. 

This is how it was used in BiomeJS:

```rust
parse_any_expression(p).or_add_diagnostic(p, expected_expression);
```

---

_@MichaReiser reviewed on 2024-09-04 12:24_

---

_@AlexWaygood reviewed on 2024-09-04 12:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:405 on 2024-09-04 12:29_

Nice, I like the idea of the `ResolvedType` enum a lot!

---

_@AlexWaygood reviewed on 2024-09-04 13:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:495 on 2024-09-04 13:06_

@MichaReiser this is a bit less generalised than what you suggested but my thinking is that we could easily generalise it in the future when we have a need for it. I'm also happy to do something more generalised now, though, if you prefer!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:476 on 2024-09-04 13:22_

Nit: I would not call this `Result` because it isn't a `std::result::Result`. Maybe `IterateType`? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:479 on 2024-09-04 13:24_

I would use an `enum` here because `iterable_ty` is misleading when it is actually a non-iterable ty.

```rust
enum IterateType<'db> {
	Iterable { element_ty: Type<'db> },
	NonIterable { ty: Type<'db> }
}
```

It also makes it more obvious that only one type is used at a time. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:405 on 2024-09-04 13:24_

I think this comment is now outdated.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:429 on 2024-09-04 13:24_

We no longer need the inner function, now that there's no monomorphization happening in the function

---

_@MichaReiser approved on 2024-09-04 13:25_

---

_@AlexWaygood reviewed on 2024-09-04 13:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:405 on 2024-09-04 13:26_

thanks, sorry for missing that

---

_@AlexWaygood reviewed on 2024-09-04 13:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:429 on 2024-09-04 13:41_

I kept the inner function for simplicity and readability, since there are several places where we return early, and it seemed easier with the question mark operator. But it probably doesn't add much any more, you're right.

---

_Merged by @AlexWaygood on 2024-09-04 14:19_

---

_Closed by @AlexWaygood on 2024-09-04 14:19_

---

_Branch deleted on 2024-09-04 14:19_

---

_Comment by @carljm on 2024-09-04 15:19_

I don't much like the callback or the new iteration-specific returned enum. What I would prefer is a more general inference-context object that gets passed into all inference methods on `Type` and has the ability to emit a diagnostic. Then `iterate` itself can emit a consistent diagnostic.

---
