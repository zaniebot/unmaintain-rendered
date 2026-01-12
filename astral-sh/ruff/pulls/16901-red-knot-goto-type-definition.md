```yaml
number: 16901
title: "[red-knot] Goto type definition"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/go-to-type-definition
created_at: 2025-03-21T16:36:31Z
updated_at: 2025-04-02T15:11:12Z
url: https://github.com/astral-sh/ruff/pull/16901
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Goto type definition

---

_@MichaReiser_

## Summary

Implement basic *Goto type definition* support for Red Knot's LSP.

This PR also builds the foundation for other LSP operations. E.g., Goto definition, hover, etc., should be able to reuse some, if not most, logic introduced in this PR. 

The basic steps of resolving the type definitions are:

1. Find the closest token for the cursor offset. This is a bit more subtle than I first anticipated because the cursor could be positioned right between the callee and the `(` in `call(test)`, in which case we want to resolve the type for `call`. 
2. Find the node with the minimal range that fully encloses the token found in 1. I somewhat suspect that 1 and 2 could be done at the same time but it complicated things because we also need to compute the spine (ancestor chain) for the node and there's no guarantee that the found nodes have the same ancestors
3. Reduce the node found in 2. to a node that is a valid goto target. This may require traversing upwards to e.g. find the closest expression. 
4. Resolve the type for the goto target
5. Resolve the location for the type, return it to the LSP

## Design decisions

The current implementation navigates to the inferred type. I think this is what we want because it means that it correctly accounts for narrowing (in which case we want to go to the narrowed type because that's the value's type at the given position). However, it does have the downside that Goto type definition doesn't work whenever we infer `T & Unknown` because intersection types aren't supported. I'm not sure what to do about this specific case, other than maybe ignoring `Unkown` in Goto type definition if the type is an intersection?

## Known limitations

* Types defined in the vendored typeshed aren't supported because the client can't open files from the red knot binary (we can either implement our own file protocol and handler OR extract the typeshed files and point there). See https://github.com/astral-sh/ty/issues/77
* Red Knot only exposes an API to get types for expressions and definitions. However, there are many other nodes with identifiers that can have a type (e.g. go to type of a globals statement, match patterns, ...). We can add support for those in separate PRs (after we figure out how to query the types from the semantic model). See https://github.com/astral-sh/ty/issues/144
* We should have a higher-level API for the LSP that doesn't directly call semantic queries. I intentionally decided not to design that API just yet.


## Test plan

https://github.com/user-attachments/assets/fa077297-a42d-4ec8-b71f-90c0802b4edb

Goto type definition on a union

<img width="1215" alt="Screenshot 2025-04-01 at 13 02 55" src="https://github.com/user-attachments/assets/689cabcc-4a86-4a18-b14a-c56f56868085" />



Note: I recorded this using a custom typeshed path so that navigating to builtins works.


---

_@MichaReiser reviewed on 2025-03-21 16:40_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/document/text_document.rs`:117 on 2025-03-21 16:40_

This is a bugfix that I copied over from ruff-server

---

_Comment by @github-actions[bot] on 2025-03-21 16:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-21 16:43_

---

_Comment by @github-actions[bot] on 2025-03-21 16:53_

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

_@MichaReiser reviewed on 2025-04-01 09:22_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/document/range.rs`:1 on 2025-04-01 09:22_

The main change here is that I extracted a helper to convert a LSP `Position` to a `TextSize` and I then used this logic in `to_text_range` (which simplifies the code quiet a bit)

---

_@MichaReiser reviewed on 2025-04-01 09:23_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:252 on 2025-04-01 09:23_

@BurntSushi I hope you're fine with me making this method public. I need a way to say: Yes, I printed this diagnostic, please don't panic ;)

https://github.com/astral-sh/ruff/blob/54c359b8fc5cc65e41d73f5a4573b5b1a93ca4b0/crates/red_knot_ide/src/goto.rs#L857-L884

---

_Marked ready for review by @MichaReiser on 2025-04-01 11:06_

---

_Review requested from @carljm by @MichaReiser on 2025-04-01 11:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-01 11:06_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-01 11:06_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-01 11:06_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-04-01 11:06_

---

_@BurntSushi reviewed on 2025-04-01 14:14_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:252 on 2025-04-01 14:14_

I was kinda hoping that we wouldn't export this, but I think it's fine. The panic is ultimately a developer aide, and it should be okay I think for programmers to opt into disabling it.

---

_@MichaReiser reviewed on 2025-04-01 14:17_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:252 on 2025-04-01 14:17_

I can also change the code to create the `source` diagnostic in a helper if you prefer. I guess, the most idiomatic way to write this is to have a `GotoTypeDefinitionDiagnostic` struct that *implements* `into_diagnostic`. 

---

_@BurntSushi reviewed on 2025-04-01 14:26_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:252 on 2025-04-01 14:26_

Oh I see. OK, I actually looked at how you're using this and see what you mean.

I think for this particular example, one thing that would be nice is to define a `Diagnostic::ignore`/`SubDiagnostic::ignore` method that consumes `self` and calls the crate internal `printed()` method. That way, it's a declarative and compiler enforced way of "throwing away" a diagnostic intentionally.

It probably doesn't cover everything that the more flexible `printed()` method does, but I think it's probably nicer if we can get away with it.

---

_@MichaReiser reviewed on 2025-04-01 14:29_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:252 on 2025-04-01 14:29_

I rewrote it so that the `source` gets recreated everytime (by having a specific `GotoTypeDefinitionDiagnostic`) but I also like your idea of having an `ignore` method which "explicitly" drops a diagnostic (which will be something we need for Ruff where `Diagnostic`s get created even if they're suppressed and are then filtered out at a later stage)

---

_Comment by @carljm on 2025-04-01 16:42_

> it does have the downside that Goto type definition doesn't work whenever we infer `T & Unknown` because intersection types aren't supported. I'm not sure what to do about this specific case, other than maybe ignoring `Unkown` in Goto type definition if the type is an intersection?

I do think that we should ideally drill down into both unions and intersections and filter out a number of types: `Any`, `Unknown`, `Todo` (so all `Type::Dynamic`), as well as `Type::AlwaysTruthy`, `Type::AlwaysFalsy`, and possibly others. Basically any type that commonly arises from narrowing that has no useful definition target. This should allow goto features to work better for many narrowed types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:792 on 2025-04-01 17:32_

Should we implement `focus_range` and `full_range` on `Type` (and return `None` for types where we don't know it) so we don't have to expose so many type internals?

---

_Review comment by @carljm on `crates/ruff_python_parser/src/lib.rs`:658 on 2025-04-01 17:36_

I don't really know the parser code so feel free to ignore, but I don't really understand this comment. Tokens are a stream, not a tree, so what does "spine to the root" even mean in terms of tokens?

---

_Review comment by @carljm on `crates/ruff_python_parser/src/lib.rs`:666 on 2025-04-01 17:37_

The use of "node" here feels odd to me, why not just say "token"?

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:150 on 2025-04-01 17:51_

If this is a subclass-of a class type (not dynamic), then it should have the navigation targets of that class.

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:161 on 2025-04-01 17:52_

Why not handle this the same as union and collect all navigation targets from elements?

(I think we can ignore negative elements.)

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:142 on 2025-04-01 17:53_

These should probably just not have any navigation targets?

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:154 on 2025-04-01 17:55_

I suppose this might as well go to the definition of `tuple`. (I realize that wouldn't work yet because we don't support typeshed yet, but it would be consistent with the way we handle strings, ints, etc above)

---

_@carljm approved on 2025-04-01 17:56_

Looks good to me! I don't know the LSP or parser code well, so I can't claim to have done a solid review of those parts, but I looked over the goto stuff.

---

_@carljm reviewed on 2025-04-01 17:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:792 on 2025-04-01 17:57_

(Oh, this comment was before I read the full diff; I've seen now the navigation-targets trait. I understand why we want to keep all the trait implementations of that in one place, but it still does feel odd to me that we do that outside of the semantics crate, meaning we have to make so many type internals public.)

---

_@AlexWaygood reviewed on 2025-04-01 18:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_ide/src/lib.rs`:150 on 2025-04-01 18:01_

you'll want to do something like

```rs
            Type::SubclassOf(subclass_of_type) => match subclass_of_type.subclass_of() {
                ClassBase::Class(class) => class.navigation_targets(db),
                ClassBase::Dynamic(_) => NavigationTargets::empty(),
            }
```

---

_@MichaReiser reviewed on 2025-04-01 19:49_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/lib.rs`:161 on 2025-04-01 19:49_

The difference is that the value isn't the type of any of the elements in the intersection. Instead, it's the type that intersects with all elements. So I think it's just wrong? I can try and see what typescript does here

---

_@carljm reviewed on 2025-04-01 20:18_

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:161 on 2025-04-01 20:18_

It's the type of _all_ the elements of the intersection; if anything it seems even more correct here than in the union case? In the union case, the actual value only needs to be one of the union elements, the other ones are all "wrong".

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session/capabilities.rs`:45 on 2025-04-02 02:54_

What's the reason for querying client capabilities from `declaration`? Shouldn't it be from `type_definition`? https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#typeDefinitionClientCapabilities, https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_typeDefinition suggests the same

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:598 on 2025-04-02 03:05_

It would be useful to add a couple of test cases for this new method.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:584 on 2025-04-02 03:06_

```suggestion
            // No token found that starts exactly at the given offset. But it's possible that
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:685 on 2025-04-02 03:12_

nit: maybe we can use the same logic as `TokenAt::Between` to avoid the `mem::replace` all for every iteration?

```suggestion
        match self {
            TokenAt::None => None,
            TokenAt::Single(token) => {
                *self = TokenAt::None;
                Some(token)
            }
            TokenAt::Between(first, second) => {
                *self = TokenAt::Single(second);
                Some(first)
            }
        }
```

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/session/capabilities.rs`:12 on 2025-04-02 03:13_

I think this should be `type_definition_link_support`? Refer to my other comment.

---

_Review comment by @dhruvmanila on `crates/red_knot_server/src/server/api/requests/goto_type_definition.rs`:65 on 2025-04-02 03:16_

Should we use `GotoDefinitionResponse::Scalar` when there's only one location? Do you know or found anything that the client might do different when it's an array v/s a location?

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3497 on 2025-04-02 03:22_

Note that the `target_range` method has been mainly used for logging purposes. From what I remember, it always tries to get the identifier range except for in certain cases like unpacking, except clause, etc.. I guess it should be fine here but thought to point it out.

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/lib.rs`:163 on 2025-04-02 03:32_

Should this go to the `Callable` definition in `collections.abc`? Pyright doesn't do it thought.

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/lib.rs`:201 on 2025-04-02 03:33_

Should this be the entire module range?

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/goto.rs`:10 on 2025-04-02 03:38_

nit: `goto_type_definition` as "goto" spelling is being used everywhere?

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/goto.rs`:243 on 2025-04-02 03:53_

These test cases are really great, interesting use of the new diagnostic system

---

_@dhruvmanila approved on 2025-04-02 03:56_

This looks really great!

This shouldn't block merging the PR but I want to look at the covering node logic tomorrow morning and also want to play around with it in an editor context.

---

_@MichaReiser reviewed on 2025-04-02 08:07_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/lib.rs`:161 on 2025-04-02 08:07_

TypeScript doesn't support *goto type definition* on intersections, which matches my understanding unless Red Knot's intersection types work differently.

The LSP shows one location for each union variant, which very nicely matches how unions work: The value is one of the types shown (see screenshot above). 

It would be very surprising if we showed intersections exactly the same as unions because users would then be unable to distinguish them from unions. I'll leave this as is (minus filtering out `Unknown`) because I'm not convinced that presenting them the same as unions is the right solution and there isn't an obvious alternative.



---

_Comment by @MichaReiser on 2025-04-02 08:08_

> I do think that we should ideally drill down into both unions and intersections and filter out a number of types: Any, Unknown, Todo (so all Type::Dynamic), as well as Type::AlwaysTruthy, Type::AlwaysFalsy, and possibly others. Basically any type that commonly arises from narrowing that has no useful definition target. This should allow goto features to work better for many narrowed types.

This already happens for unions (because those types have no navigation targets). I can see if I can refine intersection types further.

---

_@MichaReiser reviewed on 2025-04-02 08:44_

---

_Review comment by @MichaReiser on `crates/red_knot_server/src/server/api/requests/goto_type_definition.rs`:65 on 2025-04-02 08:44_

I didn't notice any difference and r-a also uses `Array` even for single values. 

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/lib.rs`:163 on 2025-04-02 08:47_

Wouldn't that be Goto definition? If so, I thin it should go to `type` (because this is what `to_meta_type` returns?

---

_@MichaReiser reviewed on 2025-04-02 08:47_

---

_@MichaReiser reviewed on 2025-04-02 08:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:792 on 2025-04-02 08:56_

I agree that it would be great to avoid exposing these internals. I called this out in the summary that we should introduce a new high level API for all those operations. I'm reluctant to do so just now because it's unclear to me how that API would look like. 

I'm getting the sense that we lack two APIs:

* Give a type query its definition: This is roughly what the big match statement does. 
* Given a node, query its definition: We'll need this for hover (because most IDEs show the signature and not just the type) and to support some of the goto targets that we currently can't retrieve a type for.

I also don't consider this a problem specific to semantic indexing. Ideally, the LSP and linter would limit their use of salsa APIs (including APIs from `ruff_db`, e.g. `system_path_to_file`). But I'm not sure yet how that would look like. I need some more code to identify a pattern.

---

_@MichaReiser reviewed on 2025-04-02 08:58_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/lib.rs`:201 on 2025-04-02 08:58_

Possibly. It makes it a bit more awkward to implement because I then need to query the source text (it's not hard). But it's not clear to me if that's more accurate because the module doesn't strictly have a range. It's just the file. That's why I opted for the most trivial solution for now. We can still adjust the ranges if it turns out that we should highlight the entire module

---

_@MichaReiser reviewed on 2025-04-02 09:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:792 on 2025-04-02 09:00_

For example, r-a requires that the ide crate uses its `Semantics` API 

---

_@MichaReiser reviewed on 2025-04-02 11:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:792 on 2025-04-02 11:00_

Okay, I have an idea of how to address some of the concerns. But I'll do so in a separate PR

---

_Merged by @MichaReiser on 2025-04-02 12:12_

---

_Closed by @MichaReiser on 2025-04-02 12:12_

---

_Branch deleted on 2025-04-02 12:12_

---

_@dhruvmanila reviewed on 2025-04-02 14:46_

---

_Review comment by @dhruvmanila on `crates/red_knot_ide/src/lib.rs`:163 on 2025-04-02 14:46_

I was referring to a variable that's annotated using `typing.Callable` similar to how we go to `int` class for a variable annotated as `int`. For example, `x: Callable[[], None] = lambda: None`.

---

_@carljm reviewed on 2025-04-02 15:11_

---

_Review comment by @carljm on `crates/red_knot_ide/src/lib.rs`:163 on 2025-04-02 15:11_

Yeah I think going to the definition of the Callable type for something with a type of `Callable` would make sense and be consistent with goto-type-definition. But also doesn't really matter until we support navigating to typeshed anyway.

---
