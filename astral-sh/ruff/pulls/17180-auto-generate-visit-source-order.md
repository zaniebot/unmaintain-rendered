```yaml
number: 17180
title: "Auto generate `visit_source_order`"
type: pull_request
state: merged
author: Glyphack
labels:
  - internal
assignees: []
merged: true
base: main
head: autogenerate-visit-source-order
created_at: 2025-04-03T15:08:54Z
updated_at: 2025-04-20T13:03:23Z
url: https://github.com/astral-sh/ruff/pull/17180
synced_at: 2026-01-10T19:33:02Z
```

# Auto generate `visit_source_order`

---

_Pull request opened by @Glyphack on 2025-04-03 15:08_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

part of: #15655 

I tried generating the source order function using code generation. I tried a simple approach, but it is not enough to generate all of them this way.

There is one good thing, that most of the implementations are fine with this. We only have a few that are not. So one benefit of this PR could be it eliminates a lot of the code, hence changing the AST structure will only leave a few places to be fixed.

The `source_order` field determines if a node requires a source order implementation. If it’s empty it means source order does not visit anything.

Initially I didn’t want to repeat the field names. But I found two things:
- `ExprIf` statement unlike other statements does not have the fields defined in source order. This and also some fields do not need to be included in the visit. So we just need a way to determine order, and determine presence.
- Relying on the fields sounds more complicated to me. Maybe another solution is to add a new attribute `order` to each field? I'm open to suggestions.
But anyway, except for the `ExprIf` we don't need to write the field names in order. Just knowing what fields must be visited are enough.

Some nodes had a more complex visitor:

`ExprCompare` required zipping two fields.

`ExprBoolOp` required a match over the fields.

`FstringValue` required a match, I created a new walk_ function that does the match. and used it in code generation. I don’t think this provides real value. Because I mostly moved the code from one file to another. I was tried it as an option. I prefer to leave it in the code as before.

Some visitors visit a slice of items. Others visit a single element. I put a check on this in code generation to see if the field requires a for loop or not. I think better approach is to have a consistent style. So we can by default loop over any field that is a sequence.

For field types `StringLiteralValue` and `BytesLiteralValue` the types are not a sequence in toml definition. But they implement `iter` so they are iterated over. So the code generation does not properly identify this. So in the code I'm checking for their types.

## Test Plan

All the tests should pass without any changes.
I checked the generated code to make sure it's the same as old code. I'm not sure if there's a test for the source order visitor.

<!-- How was it tested? -->


---

_Label `internal` added by @AlexWaygood on 2025-04-03 15:12_

---

_Comment by @Glyphack on 2025-04-03 15:43_

Hey @dcreager what do you think about this? there is some work left but I'll finish them. Just sending it now to get feedback.

---

_Comment by @github-actions[bot] on 2025-04-03 15:49_

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

_Comment by @dcreager on 2025-04-03 16:55_

Thanks for tackling this, this is exactly the kind of change I was hoping the auto-generation script would enable.

> The `source_order` field determines if a node requires a source order implementation. If it’s empty it means source order does not visit anything.
> 
> Initially I didn’t want to repeat the field names. But I found two things:
> 
> * `ExprIf` statement unlike other statements does not have the fields defined in source order. This and also some fields do not need to be included in the visit. So we just need a way to determine order, and determine presence.
> 
> * Relying on the fields sounds more complicated to me. Maybe another solution is to add a new attribute `order` to each field? I'm open to suggestions.
>   But anyway, except for the `ExprIf` we don't need to write the field names in order. Just knowing what fields must be visited are enough.

I think it's worth finding a way to not have so much repetition between the field list and this new `source_order` field. Most types want to include all fields in the source order walk, in the same order they appear in the field list. So that should be the default, if `source_order` is missing.

For types that need special handling (like `ExprBoolOp`) you could instead have `custom_source_order = true`.

For fields that shouldn't be included in the walk, you could have a field-level `skip_source_order = true`.

And for the types whose fields aren't in order, can we just reorder the field list to be in source order? Is there anything else that depends on the current field ordering for e.g. `ExprIf`? If so (and therefore we can't reorder), then you could have `custom_source_order = ["field", "list"]`.  (Or maybe a different name if you don't want to conflict with the first option mentioned above)

---

_Comment by @MichaReiser on 2025-04-03 16:59_

I like the idea of `custom_source_order = true`. I think it's fine to hand write the visitor for the few nodes that have fields that need to be skipped or require visiting in a custom ordering. 

The main purpose of the source code generators is to avoid boilerplate. Our goal isn't to write a full meta programming language :)

---

_Comment by @Glyphack on 2025-04-08 15:15_

Thanks for the ideas, I added `skip_source_order` for fields & `custom_source_order` for nodes. The toml looks less messy now.

I documented the new fields and source order generation.

For `ExprIf` I updated the field order. The snapshots required an update so I updated them.

(Sorry for the long delay I'll keep it shorter for next round)

---

_Marked ready for review by @Glyphack on 2025-04-08 20:28_

---

_Review requested from @MichaReiser by @Glyphack on 2025-04-08 20:28_

---

_Review requested from @dhruvmanila by @Glyphack on 2025-04-08 20:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:212 on 2025-04-09 06:06_

Whatäs the reason that we need to specify `source_order` here. It seems to be in the same order as `fields`.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:206 on 2025-04-09 06:07_

I'd be inclined to name the field `skip_visit` so that it can be shared between source order and the semantic order visitor. Or are there cases where we want to skip a field only in one but not the other (but maybe that's reason enough to hand write the visitor)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:224 on 2025-04-09 06:08_

We could make the generator script more clever and decide to automatically skip fields where the type is a primitive (`u32`, `bool`, `Box<str>`). That should remove the need for almost all `skip_source_order` attributes

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:353 on 2025-04-09 06:09_

I'd prefer to keep the field order as is because it represents the semantic order of the fields and instead override the order with `source_order`. This should also reduce the size of this diff

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:490 on 2025-04-09 06:10_

I'd be fine to teach the script to skip `ExprContext` and `Name` by default.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/source_order.rs`:579 on 2025-04-09 06:12_

I'd prefer not to make changes to the source order visitor as part of this PR because changes to the visitor may break existing code (the code here should call `enter_node` and `exit_node` for every visited node and visitors that override those methods may now behave differently because they see additional nodes).

---

_@MichaReiser requested changes on 2025-04-09 06:14_

Nice. This overall looks good to me. I've a few smaller inline suggestions. 

I think we should revert the AST field order changes and the changes to the `SourceOrderVisitor`. This PR should exclusively be around replacing the existing code without changing any semantics (or how structs are laid out in memory). 



---

_Review requested from @carljm by @Glyphack on 2025-04-09 17:12_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-04-09 17:12_

---

_Review requested from @sharkdp by @Glyphack on 2025-04-09 17:12_

---

_Review requested from @dcreager by @Glyphack on 2025-04-09 17:12_

---

_@Glyphack reviewed on 2025-04-09 17:13_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/visitor/source_order.rs`:579 on 2025-04-09 17:13_

Reverted these. I set custom_source_order for ExprDict and ExprFstring.

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-09 17:13_

---

_@Glyphack reviewed on 2025-04-09 17:14_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:490 on 2025-04-09 17:14_

I agree. After these only `IpyEscapeKind` and `Number` was remaining. I felt it's good to move those types to code to, so I removed the `skip_visit`. What do you think? I'm okay with adding it back.

---

_@Glyphack reviewed on 2025-04-09 17:15_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:212 on 2025-04-09 17:15_

My mistake. Removed them.

---

_Review request for @carljm removed by @AlexWaygood on 2025-04-09 17:15_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-09 17:15_

---

_Review request for @dcreager removed by @AlexWaygood on 2025-04-09 17:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:92 on 2025-04-10 09:25_

What's the meaning of `in_annotation`. Could we add some documentation for it.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:798 on 2025-04-10 09:27_

Could we change this to only include the exceptions. Most names can be derived directly from the type's name by converting to snake case.

It would also be nice if there's a way to specify those names without having to touch the generator script. 

This also makes me wonder. Is this, sort of, the inverse of `skip_source_order` because we need an entry for every type except the types in `skip_source_order`. Could we use this in some way to simplify the configuration/ make it easier to extend in the future.

---

_@MichaReiser approved on 2025-04-10 09:31_

This, overall, looks good. One more inline comment about `skip_source_order` and, what I think, its overlap with `type_to_visitor_function`

---

_@MichaReiser approved on 2025-04-10 09:31_

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:798 on 2025-04-10 13:17_

First off, this mapping is proportional to the number of types that fields can have, not proportional to the number of fields themselves or syntax nodes, so it's less worrisome to spell out all of the options. We already do that, for instance, in the definition of `SourceOrderVisitor`, since we have to explicitly list all of the `visit_[foo]` methods that a consumer might want to implement.

That said, I agree with Micha's suggestion to make this mapping only include the exceptions if possible.

---

There's also another option, which I don't know yet if I _like_. But I want to write it up to see what I feel about it.

You could also do this with a trait over in `source_order.rs`, that is implemented for each possible field type, and encodes how to turn that into a method call on the visitor. So something like:

```rust
trait FieldVisitor {
    fn visit<'a, V: SourceOrderVisitor<'a>>(&'a self, visitor: &mut V);
}

impl FieldVisitor for Decorator {
    fn visit<'a, V: SourceOrderVisitor<'a>>(&'a self, visitor: &mut V) {
        visitor.visit_decorator(self);
    }
}

/* and so on */
```

Things I don't like about this suggestion:

- More verbose than the mapping you've defined in the generator script.

Things I do like:

- Less verbose in the for-loop in the generator script (see below).
- "How to visit each field" is defined right next to the `SourceOrderVisitor` trait itself, making it less likely they'll get out of sync.
- Only have to touch this trait and its impls when you add a new field _type_, not when you add a new syntax node or field.
- I personally find it a bit easier to understand, since you can see the Rust code directly. And the logic becomes very straightforward: for each field, we call its `FieldVisitor::visit` method.

What do others think?

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:836 on 2025-04-10 13:19_

The trait would let you get rid of all of this logic (except for `is_annotation`, I think, since that's overriding how an `Expr` is visited):

```suggestion
                if field.is_annotation:
                    body += f"""
                            visitor.visit_annotation({field.name});
                      """
                else:
                    body += f"{field.name}.visit(visitor);\n"
```

---

_@dcreager reviewed on 2025-04-10 13:24_

---

_@MichaReiser reviewed on 2025-04-10 16:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:798 on 2025-04-10 16:54_

> What do others think?

That's an interesting trick.

Would the trait implementations be code-gened or hand written?


This is interesting. What I dislike about it is that it adds "accidental" complexity. It's something that's only necessary because we codegen the visitor. 

I think I have a slight preference to keep the complexity in `generate.py` over spilling it into the generated nodes (and increase comp time a little)


I do like the things that you call out as positives :)

---

_@Glyphack reviewed on 2025-04-10 17:55_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:92 on 2025-04-10 17:55_

Right, I forgot about this one. This is only needed to determine the visitor function. For other expressions we use `visit_expr` but for annotations we have `visit_annotation`.
This could go to the generate script as well since we don't have a lot of annotations and it's not gonna change probably. WDYT?

---

_@MichaReiser reviewed on 2025-04-10 17:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:92 on 2025-04-10 17:58_

Should this also be true for `StmtAnnAssign`, `TypeAlias` and other statements or is it just this one?

---

_@Glyphack reviewed on 2025-04-10 18:17_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/generate.py`:798 on 2025-04-10 18:17_

I like this idea too. One other thing that made the visitors hard without touching the code here was that some visit a slice of that type(like `visit_decorators`) and some visit a singular type.

I'm probably wrong but I feel this change is overall useful even if there was no code generation.
But the diff will probably be huge for this one. I can do another PR and see how does the code end up if we apply this change.

My reply to first comment starts here:

> This also makes me wonder. Is this, sort of, the inverse of skip_source_order because we need an entry for every type except the types in skip_source_order. Could we use this in some way to simplify the configuration/ make it easier to extend in the future.

@MichaReiser are you suggesting to generate the visitor name from the field name?
And for field types that are not straightforward have this table, so we remove most of the entries in the table.
Sorry I didn't get what does it have to do with `skip_source_order`.

I initially created the table for visitors that visit a sequence. I wanted to mark what visitors require a for loop.
Although for some like `visit_decorators` you can infer it's visiting a seq by the "s" at the end for `alias` it's not the case.

I'm thinking about this idea, to determine the visitor name:

1. Lookup the table (for exceptional cases)
2. Convert the name to snake_case and build the visitor
3. Infer if it's visiting sequence by the `s` in the end

Hopefully the point 3 can be gone after I send a refactor either just making it consistent or applying Douglas's suggestion.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/ast.toml`:446 on 2025-04-10 19:43_

I think this would be `# Because BytesLiteralValue type ...` ?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/generated.rs`:7323 on 2025-04-10 19:49_

I think this should be `visit_annotation`

---

_@dhruvmanila reviewed on 2025-04-10 19:53_

---

_@MichaReiser reviewed on 2025-04-11 08:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:798 on 2025-04-11 08:22_

I think it would be great if we can explore this idea as a separate PR, unless it creates a lot of additional work.

---

_@MichaReiser reviewed on 2025-04-11 11:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:798 on 2025-04-11 11:49_

Having said that. I think I'm also fine landing this PR as is and creating a separate PR to simplify this `type_to_visitor_function`. I let you and @dcreager decide on what you prefer. @dcreager can you go ahead and merge this PR if you're happy with it because I'll be out next week.

---

_Review comment by @Glyphack on `crates/ruff_python_ast/src/generated.rs`:7323 on 2025-04-11 12:31_

Thanks, corrected.

---

_@Glyphack reviewed on 2025-04-11 12:31_

---

_@Glyphack reviewed on 2025-04-11 12:38_

---

_Review comment by @Glyphack on `crates/ruff_python_ast/ast.toml`:92 on 2025-04-11 12:38_

Fixed it for `StmtAnnAssign`. For `TypeAlias` we did not have it before?

https://github.com/astral-sh/ruff/pull/17180/files#diff-97bb55be932e9d4bbd9219dfd4d62507ae171c8b642a3feceeceeb7fd4d1c6a6L121

But you are right for value it should be annotation. I changed it to annotation.

---

_@MichaReiser reviewed on 2025-04-11 12:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/ast.toml`:92 on 2025-04-11 12:52_

My preference is to preserve the behavior from the hand written visitor and to change the visiting (if that's indeed the case) in a separate PR

---

_Review comment by @dcreager on `crates/ruff_python_ast/generate.py`:798 on 2025-04-15 13:47_

I'm not a fan of inferring behavior from the presence of an `s` at the end of the name — that's seem too magical for what we're trying to accomplish here.  If that's what we would have to do to avoid the table, I'd rather keep the table.

:+1: to landing this as-is and iterating on possible changes in later PRs

---

_Review comment by @dcreager on `crates/ruff_python_parser/tests/snapshots/valid_syntax@ambiguous_lpar_with_items_if_expr.py.snap`:4 on 2025-04-15 13:49_

Do we know where these edits to the snapshot files are coming from? They don't look relevant to the changes you've made to the generation script

---

_@dcreager reviewed on 2025-04-15 13:49_

---

_@Glyphack reviewed on 2025-04-15 15:54_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/tests/snapshots/valid_syntax@ambiguous_lpar_with_items_if_expr.py.snap`:4 on 2025-04-15 15:54_

This is happening depending on what version of cargo-insta you have locally when running the tests(more context https://github.com/mitsuhiko/insta/issues/676). I'll go ahead and revert back to keep it tidy.

---

_@Glyphack reviewed on 2025-04-15 15:57_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/tests/snapshots/valid_syntax@nested_async_comprehension_py310.py.snap`:10 on 2025-04-15 15:57_

This shouldn't happen? checking..

---

_@Glyphack reviewed on 2025-04-15 16:02_

---

_Review comment by @Glyphack on `crates/ruff_python_parser/tests/snapshots/valid_syntax@nested_async_comprehension_py310.py.snap`:10 on 2025-04-15 16:02_

Oh I needed a rebase.

---

_@dcreager approved on 2025-04-17 12:59_

Perfect, thanks for addressing the snapshot changes!

As mentioned upthread, we can look at other simplifications as follow-on PRs.  Happy to merge this as-is.

---

_Merged by @dcreager on 2025-04-17 12:59_

---

_Closed by @dcreager on 2025-04-17 12:59_

---

_Branch deleted on 2025-04-20 13:03_

---
