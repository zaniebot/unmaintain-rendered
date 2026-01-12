```yaml
number: 11135
title: "[red-knot] add more types"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot/more-types
created_at: 2024-04-24T21:42:49Z
updated_at: 2024-04-26T20:05:59Z
url: https://github.com/astral-sh/ruff/pull/11135
synced_at: 2026-01-12T15:55:34Z
```

# [red-knot] add more types

---

_@carljm_

Fill out the representation of types to a few more cases (especially unions and intersections) before going too far with type evaluation.



---

_Review requested from @MichaReiser by @carljm on 2024-04-24 21:42_

---

_Review requested from @AlexWaygood by @carljm on 2024-04-24 21:42_

---

_Comment by @github-actions[bot] on 2024-04-24 21:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:45 on 2024-04-25 06:32_

What's an MRO? Can we add a link or spell the word out?

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:46 on 2024-04-25 06:32_

It's a bit confusing that the type is called `ClassType` but the variant is `Instance`. I think I would align the two.

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:43 on 2024-04-25 06:33_

Nit: Use `///` for member documentation. `//` is for inline comments
```suggestion
    /// a specific function
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:56 on 2024-04-25 06:35_

I associate the never type with the `!` type in rust or `never` in TypeScript where it indicates that a function never returns (e.g. it always throws). But what I understand is that this is more like a `unit` type 

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:85 on 2024-04-25 06:45_

It's a bit more annoying. But a common pattern here (see e.g. `Path`) is to have a `DisplayType` struct that implements `std::fmt::Display` and have the `display` function return that struct. It has the advantage that you can you the `display` function in `format_args` places (`write`, `format` `tracing` etc macros).

```suggestion
    fn display(&self, env: &TypeEnvironment) -> DisplayType {
        DisplayType { ty: self, environment: env }
    }
    
    #[derive(Copy, Clone, Debug)]
    struct DisplayType<'a> {
    	ty: &'a Type,
    	environment: &'a TypeEnvironment
  	}
  	
  	impl std::fmt::Display for DisplayType<'_> {
  		fn fmt(&self, &mut std::fmt::Formatter) -> std::fmt::Result {
  			if let Some(name) = self.ty.name() {
            write!(f, "{}", name)
        } else {
            match self {
                Type::Union(inner) => inner.display(f, env),
                Type::Intersection(inner) => inner.display(f, env),
                _ => Err(std::fmt::Error),
            }
        }			}
		}
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:77 on 2024-04-25 06:46_

```suggestion
            f.write_str(name)
```

Using `write_str` bypasses the somewhat heavy format args compiler infrastructure

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:82 on 2024-04-25 06:48_

I would panic here. This is a programming error. It otherwise feels like it's a conditional `Display` where it returns `NotImplemented` if you call it on a value for which it isn't supported. 

I think it's overall also unexpected because e.g. a common assumption is that writing to a `String` never fails... well, that would no longer be true. I think I would write it as

```rust
match self {
	Type::Union(inner) => inner.display(f, env), 
	Type::Intersection(inner) => inner.display(f, env),
	_ => {
		let name = self.name.expect("Expect type to have a name");
		f.write_str(name)
	}
}
```

or you make it more explicit and implement `display` (and possibly name) on each type so that `display` becomes a simple static dispatch call.

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:118 on 2024-04-25 06:51_

```suggestion
        f.write_str("(")?;
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:127 on 2024-04-25 06:52_

```suggestion
            if !first {
                f.write_str(" | ")?;
            };
            
            first = false;
            let ty = env.type_for_id(*elem_id);
            ty.display(f, env)?;
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:127 on 2024-04-25 06:54_

or you could even move the first out of the loop

```suggestion
	let mut elements = self.elements.iter();
	
	if let Some(first) = elements.next();
		let ty = env.type_for_id(*elem_id);
		ty.display(f, env)?;
	}
	
	for rest in elements {
		let ty = env.type_for_id(*elem_id);
		write!(f, " | {}", ty.display())?;
	}
		
	Ok()
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:158 on 2024-04-25 06:56_

How common are negations? We could consider boxing the `HashSet` to avoid increasing the size of all other types (this is now by far the largest type). 

This is untrue if we make `TypeEnvironment` a struct of array where we use a different arena for each type and define `TypeId` as

```rust
#[derive(Copy, Clone, Debug, Eq, PartialEq, Hash)]
enum TypeId {
	/// Singleton types first
	Any,
	Unknown,
	...,
	Function(FunctionTypeId),
	Class(ClassTypeId),
	...
}
```

I think that might be overall interesting to explore because: 
* We don't need to store singleton types. We can just construct them anew everytime they're used (no need to write them to an arena)
* Increasing a single type is less problematic because it doesn't increase the size of all type elements
	

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:158 on 2024-04-25 06:56_

What happens if a type is both in the positive and in the negative set? Is that possible? Should that be disallowed? 

---

_@MichaReiser approved on 2024-04-25 06:56_

---

_@carljm reviewed on 2024-04-25 12:20_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:45 on 2024-04-25 12:20_

method resolution order -- the linearized list of base classes for a class. I'll spell it out.

---

_@carljm reviewed on 2024-04-25 12:26_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:56 on 2024-04-25 12:26_

No, this is like the `!` type in Rust or `never` in typescript: it's the bottom type. It can be used as a return annotation to indicate that a function never returns, but there are other cases where it can show up; it tends to indicate dead code, because for an expression to have the bottom type means that there is no possible value for that expression. This is what "the empty set of values" comment means. In a set-theoretic model, a type can be considered as representing a set of possible values, and the never type represents "no possible values." This is not the same thing as Rust's unit type: that's a type with exactly one possible value. (In that sense, unit type is more like the type of `None` in Python, a type whose only possible value is `None`. And much like Rust's unit type, in Python we spell both the type and the value the same way).

---

_@carljm reviewed on 2024-04-25 12:27_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:85 on 2024-04-25 12:27_

This makes a ton of sense, thank you!

You don't have to take the time to write out the code for a suggestion like this, unless you want to :) It's clear enough from the text.

---

_@AlexWaygood reviewed on 2024-04-25 12:32_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:46 on 2024-04-25 12:32_

Or you could have something like

```suggestion
    Instance { instance_of: ClassType },
```

---

_@carljm reviewed on 2024-04-25 12:40_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:127 on 2024-04-25 12:40_

I like the first suggestion; don't like duplicating most of the contents of the loop

---

_@AlexWaygood reviewed on 2024-04-25 12:40_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:54 on 2024-04-25 12:40_

Any, Unknown and Unbound seem to all be types that represent dynamic or unsafe typing in some way. That reminds me of this in the mypy codebase (which is really an enum, though it doesn't use Python's `enum` module, I think for performance and/or mypyc reasons): https://github.com/python/mypy/blob/82ebd866830cd79e25bf9d59e9f9474bd280c4f5/mypy/types.py#L187

I wonder if it might make sense to have a single Dynamic variant here:

```suggestion
    Dynamic(DynamicType),
```

And have a separate DynamicType enum, e.g.

```rs
/// Enumeration of types that are fundamentally unsafe in some way
enum DynamicType {
    Any,
    Unbound,
    Unknown
}
```

---

_@carljm reviewed on 2024-04-25 12:44_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:158 on 2024-04-25 12:44_

It would mean the Intersection should resolve to `Never`, because if type `X` is in both positive and negative, that means the intersection specifies the set of values which must be type `X` and must not be type `X`. Which is the empty set of values :) But this is not something we can represent in the type itself; we will need code to construct and simplify algebraic types, which will need to handle cases like this.

---

_@AlexWaygood reviewed on 2024-04-25 12:52_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types.rs`:128 on 2024-04-25 12:52_

I have a vague sense of unease about using an unordered collection for this field. While the elements in a union should definitely be flattened and deduplicated, I think the order of elements in a union _might_ sometimes matter in situations involving dynamic types. `Any | None` might be more dynamic than `None | Any`. But I don't have any concrete examples/I'm not sure if this is true.

---

_@carljm reviewed on 2024-04-25 12:54_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:54 on 2024-04-25 12:54_

I think the semantics of `Unbound` are quite different; it doesn't represent dynamic typing, it represents cases where we know for sure that a name either is unbound, or may be unbound (in this case we'd use a union with unbound). So I wouldn't want to include `Unbound` in such a `DynamicType` enum.

(FWIW, I'm also not totally sold on representing `Unbound` as a type at all. It will result in a lot more union types, and maybe confusing error messages too. And I don't think it fully fits into the model, since a name can be unbound, but an expression can't be. So I'm leaning towards just removing `Unbound` here and modeling it differently.)

We could have a `DynamicType` enum, but it also doesn't seem clearly necessary, so I'd rather wait and see if the type evaluation code suggests that it would be beneficial in some way.

For instance, if we want to have a strict mode where `Unknown` acts like `object`, then I think grouping `Any` and `Unknown` together like this might cause more harm than good.

It may be that at some point we want to track more fine-grained versions of `Any` like mypy does; at that point I think grouping them would make more sense.

---

_@carljm reviewed on 2024-04-25 12:57_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:128 on 2024-04-25 12:57_

I think it's pretty important that in terms of the type system, unions are not ordered. I'm not aware of anything in the spec that suggests they should be, and I think it would have really bad implications for the consistency of the type system.

There was some discussion in the ongoing intersection-PEP effort about making intersections ordered, but I think the latest conclusion there (which I think is the correct conclusion) is that intersections should not be ordered either.

My only unease here is about whether it's important for UX to feed unions that originate in type annotations back to the user (e.g. in error messages) in the same order the user specified the union in their annotation. But I'm not sure if this is important enough to warrant the extra complexity and inefficiency of e.g. having to deduplicate unions ourselves.

---

_@carljm reviewed on 2024-04-25 13:00_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:158 on 2024-04-25 13:00_

I really like this idea. It doesn't really make sense to have the singleton types even belong to a module at all; they are singletons, after all. Let me explore this.

---

_@carljm reviewed on 2024-04-25 13:10_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:46 on 2024-04-25 13:10_

Yeah, let me think about this a bit more. I think I like Alex's version. The inner struct is really a representation of a class,  but the type represents instances of that class.

---

_@carljm reviewed on 2024-04-25 13:41_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:158 on 2024-04-25 13:41_

This has a lot of interactions with my other WIP on per-module arena instead of global arena, so I'm going to explore it in the next PR instead of in this one.

---

_Merged by @carljm on 2024-04-25 13:59_

---

_Closed by @carljm on 2024-04-25 13:59_

---

_Branch deleted on 2024-04-25 13:59_

---

_@carljm reviewed on 2024-04-25 23:23_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:158 on 2024-04-25 23:23_

https://github.com/astral-sh/ruff/pull/11152

---

_@carljm reviewed on 2024-04-25 23:23_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:128 on 2024-04-25 23:23_

https://github.com/astral-sh/ruff/pull/11152 makes these ordered using IndexSet.

---

_Label `internal` added by @carljm on 2024-04-26 20:05_

---
