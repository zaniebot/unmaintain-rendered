```yaml
number: 13130
title: "[red-knot] infer basic (name-based) annotation expressions"
type: pull_request
state: merged
author: chriskrycho
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-basic-annotation-expressions
created_at: 2024-08-27T23:18:53Z
updated_at: 2024-09-03T14:25:09Z
url: https://github.com/astral-sh/ruff/pull/13130
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] infer basic (name-based) annotation expressions

---

_Pull request opened by @chriskrycho on 2024-08-27 23:18_

## Summary

- Introduce methods for inferring annotation and type expressions.
- Correctly infer explicit return types from functions where they are simple names that can be resolved in scope.

Contributes to astral-sh/ty#244 by way of helping unlock call expressions (this does not remotely finish that, as it stands, but it gets us moving that direction).

## Test Plan

Added a test for function return types which use the name form of an annotation expression, since this is aiming toward call expressions. When we extend this to working for other annotation and type expression positions, we should add explicit tests for those as well.


---

_Review requested from @carljm by @chriskrycho on 2024-08-27 23:18_

---

_Review requested from @MichaReiser by @chriskrycho on 2024-08-27 23:18_

---

_Review requested from @AlexWaygood by @chriskrycho on 2024-08-27 23:18_

---

_Label `red-knot` added by @carljm on 2024-08-27 23:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:322 on 2024-08-28 09:10_

nit: the function will always have a _return type_, even if it doesn't have a _return annotation_. If it doesn't have a return annotation, we will either just infer the function's return type as `Unknown` (what mypy does), or analyse the body of the function to figure out the return type (what pyright does)

```suggestion
    /// type inferred from this function's return annotation
    /// (if it has a return annotation)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1378 on 2024-08-28 09:13_

```suggestion
    }

```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1393 on 2024-08-28 09:15_

Maybe make this a debug assertion? I think it's unlikely it'll ever trigger; it feels more like a "sanity check" to me

```suggestion
                debug_assert!(
                    name.ctx.is_load(),
                    "name in a type expression is always 'load' but got: '{:?}'",
                    name.ctx
                );
```

---

_@AlexWaygood reviewed on 2024-08-28 09:22_

Nice! A couple of nitpicks below.

Here's some high-level thoughts that shouldn't block this PR, just things we could think about as followups:
- it might be nice if `infer_annotation_expression` took a custom `TypeExpression` enum as its input argument rather than the `ast::Expr` enum, to codify the fact that a valid type expression necessarily excludes certain Python expression nodes entirely, such as `Expr::Call`s, `Expr::Lambda`s, `Expr::Named`, etc.
- currently we're not looking at any qualifiers an annotation expression might or might not include that wrap the annotation's type expression. But in the long run, we'll want to take note of those and record them somewhere. It might make sense for `infer_annotation_expression` to return something like `(HasSet<TypeQualifier>, Type<'db>)` rather than simply `Type<'db>`.

Again, these high-level comments are just me thinking out loud about how we might want this code to work in the long run; not really anything you need to respond to in this PR!

---

_@carljm reviewed on 2024-08-28 12:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:322 on 2024-08-28 12:16_

I agree with this nit, but I have a nit on the nit 😆  although we use the term "infer" pretty broadly, I wouldn't choose to use that term when we're specifically referring to simply resolving an annotation expression to a type:
```suggestion
    /// annotated return type for this function, if any
```

---

_Comment by @carljm on 2024-08-28 12:27_

Replying to a couple of the high-level thoughts here, though I agree that neither of them should be implemented in this PR:

> * it might be nice if `infer_annotation_expression` took a custom `TypeExpression` enum as its input argument rather than the `ast::Expr` enum, to codify the fact that a valid type expression necessarily excludes certain Python expression nodes entirely, such as `Expr::Call`s, `Expr::Lambda`s, `Expr::Named`, etc.

I'm not sure about this. It implies that we would then need to have some other method that takes an `ast::Expr`, emits diagnostics for invalid forms, and returns the custom enum. But it seems to me that it will be simpler, clearer, and more efficient if that is the job of `infer_annotation_expression` itself. Why match twice when we can match once, emit diagnostics on invalid forms and perform correct resolution of valid forms?

It seems to me like this is an idea we can keep in mind if at some point things reach a level of complexity where it really concretely improves type safety (for instance, if we have built up a large collection of methods that are subsidiary to `infer_annotation_expression` and all should operate only on a valid type expression form -- but I'm not convinced this will ever happen, because I think the subsidiary methods are more likely to operate on a specific expression kind instead), but this isn't something we should be a priori aiming for, or introducing for its own sake if its only taken by a single method.

> currently we're not looking at any qualifiers an annotation expression might or might not include that wrap the annotation's type expression. But in the long run, we'll want to take note of those and record them somewhere. It might make sense for `infer_annotation_expression` to return something like `(HasSet<TypeQualifier>, Type<'db>)` rather than simply `Type<'db>`.

Yes, agreed. Probably best to sort out the details here once we are adding support for a type qualifier, so we can design it in context with how the information is used.

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:1393 on 2024-08-28 12:57_

Seems reasonable—here I was following the example of the `panic!` on `infer_function_type_params` (in a `let...else`), though in that case it's necessary simply for using the construct.

---

_@chriskrycho reviewed on 2024-08-28 12:57_

---

_@chriskrycho reviewed on 2024-08-28 12:59_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types.rs`:322 on 2024-08-28 12:59_

DOUBLE NIT. I love it. 😁 

---

_Comment by @github-actions[bot] on 2024-08-28 13:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2239 on 2024-08-29 20:48_

This isn't _quite_ right -- quoted annotations can be used for forward references in Python (something I'm currently struggling with in https://github.com/astral-sh/ruff/pull/12951), so `"hello"` might actually be a valid type annotation if `hello` resolves to a symbol in scope. See https://peps.python.org/pep-0484/#forward-references. (But you're correct that all of the others are not generally valid in type annotations. Except for inside `Literal` slices. And also `Ellipsis`, that also has some special meaning in type annotations 😄.)

---

_@AlexWaygood reviewed on 2024-08-29 20:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:181 on 2024-08-29 21:16_

I think this is too noisy to keep around at INFO level (which I believe is the default.) It might even be too noisy at DEBUG. We will look up types for lots of expressions, and looking up types is generally not an interesting operation (as opposed to, say, actually inferring the types in the first place.)

Also, it would be weird to trace `try_expression_ty` but not `expression_ty` above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:186 on 2024-08-29 21:16_

Same comment as above; I think we shouldn't instrument this. (Also because `Definition` is quite a verbose debug dump.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:195 on 2024-08-29 21:17_

This also doesn't seem worth instrumenting?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:313 on 2024-08-29 21:19_

These three infer-region calls do seem worth instrumenting, in principle, except that I think they always correspond 1:1 with calls to the Salsa queries `infer_scope_types`, `infer_definition_types`, `infer_deferred_types`, and `infer_expression_types` above. Which I think makes it mostly noise to have both? So I would remove these, too.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:323 on 2024-08-29 21:25_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:369 on 2024-08-29 21:26_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:426 on 2024-08-29 21:26_

Isn't this also duplicative of the existing span in `infer_definition_types`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:429 on 2024-08-29 21:27_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:441 on 2024-08-29 21:27_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:446 on 2024-08-29 21:29_

This again doesn't seem that useful to me, in that it will always correspond 1:1 with an `infer_scope_types` call (which is already traced), whenever that scope is a module scope. I guess it could be useful to know that the scope is a module scope, but that would be more clearly done by improving the `infer_scope_types` trace span.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:473 on 2024-08-29 21:31_

Type parameters can't be deferred, but parameter annotations can be.
```suggestion
        // TODO: this should also be applied to parameter annotations.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:528 on 2024-08-29 21:33_

I think this is too noisy also -- clearly (given the implementation) it will always correspond to a call to `infer_definition_types`, which is already traced.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:535 on 2024-08-29 21:34_

And this is just a one-liner that always calls `infer_definition`, which always calls `infer_definition_types`.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:540 on 2024-08-29 21:34_

This one also feels redundant to me with the existing tracing of `infer_definition_types`.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:574 on 2024-08-29 21:35_

nit: it's only the annotations on parameters that might be deferred, not the entire parameter.
```suggestion
            // TODO: this should also be applied to parameter annotations.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:662 on 2024-08-29 21:39_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:667 on 2024-08-29 21:39_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:709 on 2024-08-29 21:39_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:590 on 2024-08-29 21:41_

I just generally think it's too noisy to have default tracing turned on for this level of detail, particularly at `INFO`; and it seems a little arbitrary that we'd do it for parameters but not all the other nooks and crannies of the AST. IMO having trace spans by default for the various Salsa query entry points is about the right level of abstraction; if you're debugging a more detailed issue in a particular area of inference, you can add `dbg!()` calls at the points that are interesting for what you're debugging.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:612 on 2024-08-29 21:42_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:625 on 2024-08-29 21:42_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:651 on 2024-08-29 21:42_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:646 on 2024-08-29 21:43_

```suggestion
        _parameter_with_default: &ast::ParameterWithDefault,
        definition: Definition<'db>,
    ) {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1349 on 2024-08-29 21:44_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1824 on 2024-08-29 21:44_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2124 on 2024-08-29 21:45_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2253 on 2024-08-29 21:52_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2239 on 2024-08-29 21:53_

Yeah, we should move `StringLiteral` out of this section and have a dedicated comment for it `TODO: stringified annotations`, and then update this comment for the rest of them to not use a string as an example.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2119 on 2024-08-29 21:55_

I think it's likely that we should remove the universal `infer_` prefix from methods here, but looking at this now, it's something I think we want to consider carefully in its own PR, and I don't like having this PR leave us in a halfway state that sort of suggests we are committed to that direction, but requires another PR to complete it. I'd rather just stay fully consistent with the existing pattern for now.
```suggestion
    fn infer_annotation_expression(&mut self, expression: &ast::Expr) -> Type<'db> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2267 on 2024-08-29 21:56_

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2278 on 2024-08-29 21:56_

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 21:56_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2239 on 2024-08-29 21:56_

Oh, right, I forgot about that part (even though I was mucking around with it just yesterday)! Will update. I was wondering about `Ellipsis`, too. I will note that it is a “TODO” rather than a “nothing to see here” and go look up what it does.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2287 on 2024-08-29 21:57_

Update here also in accordance with the comment above about string literals and string annotations.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2134 on 2024-08-29 22:02_

I think I would prefer a different approach here, to reduce duplication. I think `infer_annotation_expression` should handle any valid annotation-expression-only forms, and just defer everything else to `infer_type_expression` blindly. Because any valid type expression is also a valid annotation expression, and we shouldn't need to double-encode information here in `infer_annotation_expression` about what is or isn't a valid type expression.

Given that as of this PR we aren't handling _any_ annotation-expression-only forms, that means that for now, `infer_annotation_expression` should be just a comment like `TODO handle annotation-expression-only special forms` and then  just defer everything to `infer_type_expression`.

Are there problems with this approach that I'm missing?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2262 on 2024-08-29 22:02_

```suggestion
    fn infer_type_expression(&mut self, expression: &ast::Expr) -> Type<'db> {
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2254 on 2024-08-29 22:05_

Given that we will in the future have standalone type expressions, I think it's important that `infer_type_expression` insert the expression type, so I think this should also move down to `infer_type_expression`.

For now I don't think we need it in `infer_annotation_expression` at all (in keeping with the above comment about making it a very light wrapper around `infer_type_expression`). In future it should insert _something_ into the map for whatever annotation-only special forms it handles; we can figure that out when the time comes to handle those special forms.

---

_@carljm requested changes on 2024-08-29 22:08_

Great work! In terms of the broad strokes, this looks excellent.

A lot of my comments are basically just a single comment that I don't think we should add such fine-grained tracing of type inference by default; if we did it everywhere, it'd be so noisy in any realistic case that you'd never be able to find anything. When debugging at that level, it works better IMO to manually add the specific `dbg!()` calls that are relevant to the case you're debugging. Open to discussing this if you think I'm missing the value of this tracing!

---

_Comment by @chriskrycho on 2024-08-29 22:46_

Ha, very strongly agreed re: tracing instrumentation – I was going to ask if you thought *any* of those were valuable to keep, and I think the answer here is basically “no”. I’ll double check on the annotation-vs.-type-expressions thing as well and respond on that specific comment.

---

_@chriskrycho reviewed on 2024-08-29 22:47_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:181 on 2024-08-29 22:47_

Agreed, I’m going to remove basically all of these; I meant to and failed to call it out on Discord earlier!

---

_@chriskrycho reviewed on 2024-08-29 22:47_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:181 on 2024-08-29 22:47_

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 22:47_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:186 on 2024-08-29 22:47_

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 22:47_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:195 on 2024-08-29 22:47_

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 22:49_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:426 on 2024-08-29 22:49_

It is! I originally added it because I wanted to see if we ended up getting this far, and so wanted to just emit an event. Then I figured out how to read the tree better. (Aside: misreading the tree of traces is why I was wrong on my 95%-confident thing earlier: we were *successfully* getting out of all the paths represented by those traces.)

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 22:49_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:446 on 2024-08-29 22:49_

```suggestion
```

---

_@chriskrycho reviewed on 2024-08-29 22:49_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:473 on 2024-08-29 22:49_

Ahhhh, good to know. 👍🏼

---

_@chriskrycho reviewed on 2024-08-29 22:50_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:620 on 2024-08-29 22:50_

This is a good thing to do, I *believe*, but it should be in the *next* PR, which will wire up very basic parameter inference.

```suggestion
        self.infer_optional_expression(parameter.annotation.as_deref());
```

---

_@chriskrycho reviewed on 2024-08-29 22:52_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2119 on 2024-08-29 22:52_

I debated on exactly that point, so fair enough. I’ll do that in a separate commit from the rest of the batch so I make sure I get everything correctly wired up with the rename refactor.

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2254 on 2024-08-29 22:53_

Ah, that makes sense, yes. Will do. 👍🏼

---

_@chriskrycho reviewed on 2024-08-29 22:53_

---

_@chriskrycho reviewed on 2024-08-29 22:59_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2239 on 2024-08-29 22:59_

Did that in a3d82a8f.

---

_@chriskrycho reviewed on 2024-08-29 23:01_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2134 on 2024-08-29 23:01_

That approach seems perfectly reasonable. I debated doing it that way initially, and simply didn’t have a strong enough sense of the tradeoff there—reviewing the grammar, though, I see what you mean. There just isn’t that much which is specific to annotation expressions.

---

_Comment by @codspeed-hq[bot] on 2024-08-29 23:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/chriskrycho:rk-basic-annotation-expressions)

### Merging astral-sh/ruff#13130 will **not alter performance**

<sub>Comparing <code>chriskrycho:rk-basic-annotation-expressions</code> (407c27e) with <code>main</code> (34b4732)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@chriskrycho reviewed on 2024-08-30 12:59_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2134 on 2024-08-30 12:59_

Done (see 6af652ce)!

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2262 on 2024-08-30 13:00_

Went back to `infer_*` in 0eaf748d. 🎉 

---

_@chriskrycho reviewed on 2024-08-30 13:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2125 on 2024-08-30 15:00_

```suggestion
                self.infer_name_expression(name).instance()
```

---

_@carljm approved on 2024-08-30 15:01_

Looks great!

---

_@chriskrycho reviewed on 2024-08-30 15:10_

---

_Review comment by @chriskrycho on `crates/red_knot_python_semantic/src/types/infer.rs`:2125 on 2024-08-30 15:10_

Ha! An artifact of *so much tracing*.

---

_Merged by @carljm on 2024-08-30 15:24_

---

_Closed by @carljm on 2024-08-30 15:24_

---

_Branch deleted on 2024-08-30 15:29_

---

_@MichaReiser reviewed on 2024-09-03 07:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:80 on 2024-09-03 07:40_

We should change this to an actual sentence rather than the method name. Tracing messages are user facing (not `trace` but we might decide to make them user facing in the future).

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:2090 on 2024-09-03 07:41_

What's the motivation for placing each method in its own impl block?

---

_@MichaReiser reviewed on 2024-09-03 07:41_

---

_@carljm reviewed on 2024-09-03 14:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:80 on 2024-09-03 14:22_

This function is a partially-broken temporary crutch just to get us through until we have fixpoint iteration and full deferred resolution of annotations in the necessary places; it should go away. So I'm not too worried about this particular trace message sticking around for a long time. But this is a good point.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2090 on 2024-09-03 14:25_

The idea was not "each method" but "each set of related methods", though at the moment it is just one method for annotation expressions and one for type expressions; that will change. It was just a way to more clearly separate inference of value expressions from inference of annotation expressions and type expressions.

This was Chris' idea and it seemed fine to me, but I'm also fine with just using comment headers to achieve the same separation.

---

_@carljm reviewed on 2024-09-03 14:25_

---
