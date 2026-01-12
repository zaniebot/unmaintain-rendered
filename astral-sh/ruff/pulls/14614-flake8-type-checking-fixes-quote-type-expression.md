```yaml
number: 14614
title: "[`flake8-type-checking`] Fixes `quote_type_expression`"
type: pull_request
state: closed
author: Daverball
labels: []
assignees: []
draft: true
base: main
head: bugfix/quote-annotation
created_at: 2024-11-26T16:19:03Z
updated_at: 2024-11-27T13:49:01Z
url: https://github.com/astral-sh/ruff/pull/14614
synced_at: 2026-01-12T15:55:48Z
```

# [`flake8-type-checking`] Fixes `quote_type_expression`

---

_@Daverball_

Fixes: #14538
Fixes: #14554

## Summary

This achieves a more feature complete version of wrapping annotations in quotes while removing nested forward references and handling nested quotes correctly.

This is currently just a proof of concept and the implementation could do with some refactoring and cleaning up. This should also additional test cases for regression tests, but there is already one existing test case showcasing that we now can correctly handle nested quotes.

It's also not currently making use of the parsed annotations cache. Instead of passing the semantic model, locator and stylizer separately, we probably should just pass the checker to these helper functions, then we can use the cached annotation parsing lookup.

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2024-11-26 16:21_

@MichaReiser Could you take a quick look at this to see if this is heading in a viable direction?

---

_Comment by @github-actions[bot] on 2024-11-26 16:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-11-26 17:42_

I don't fully understand the problem yet, so sorry if I'm way off here. 

I understand that we want to mutate the type annotation expression, and the last step is to write it into a quoted string. However, the problem is that the `Generator::quote`  is only the preferred quote and the generator might decide to pick the opposite quote if it avoids escaping. 

I wonder if we, instead, should change the `Generator::quote` to an enum with two variants: `Prefer(Quote)` and `Fixed(Quote)` where the second option is new and forces the generator to use the given quote style, even if it leads to more escapes. 

We could then use `VisitorMut` to rewrite the annotation and then unparse the expression with `Fixed(stylist.quote().opposite()`. 

This seems to me like an overall less-invasive change.


---

_Comment by @Daverball on 2024-11-26 21:23_

> I wonder if we, instead, should change the Generator::quote to an enum with two variants: Prefer(Quote) and Fixed(Quote) where the second option is new and forces the generator to use the given quote style, even if it leads to more escapes.

That sounds like a good idea, but is somewhat orthogonal to this change. So it's something we should change either way, regardless of how we remove the forward references.

Rewriting the AST was my first instinct for a proof of concept, since it seemed really easy. But I don't think it's safe unless I create a copy of all the nodes in the expression first, since otherwise other rules will encounter the transformed AST before the file has actually been fixed. And doing that copy seems really slow and inefficient, traversing the expression twice is bad enough. I haven't found any uses of `Transformer` for other fixes (if that's what you meant by `VisitorMut`? I couldn't find any other mutating AST visitors).

I think there's additional potential future benefits to extracting some of the core functionality of `Generator` into a trait. There may be other fixes that could be made more robust/general by using this as a standard way to perform cheap AST transformations while you're unparsing that very same AST.

---

_Comment by @MichaReiser on 2024-11-27 07:52_

> Rewriting the AST was my first instinct for a proof of concept, since it seemed really easy. But I don't think it's safe unless I create a copy of all the nodes in the expression first, since otherwise other rules will encounter the transformed AST before the file has actually been fixed. And doing that copy seems really slow and inefficient, traversing the expression twice is bad enough. I haven't found any uses of Transformer for other fixes (if that's what you meant by VisitorMut? I couldn't find any other mutating AST visitors).

I'm not that concerned about performance:

* Generating the fix is already the slow path. Most projects have no violations and, therefore, no fixes need generating
* It's not necessary to clone the entire AST, we can just clone the nodes that need to be rewritten, in this case the annotation. Most annotations are only a few nodes, cloning them should be reasonably fast. 

> I think there's additional potential future benefits to extracting some of the core functionality of Generator into a trait.

Possibly, but I'd prefer a more local fix if possible and we can revisit a more flexible `Generator` implementation when we have more usages for this pattern that inform our design.

---

_Comment by @Daverball on 2024-11-27 08:37_

@MichaReiser Fair enough, I will go the `Transformer` route and see how that ends up looking. I'm not familiar enough with the `Clone` trait to know what `ast::Expr.clone()` would do. How deep is the copy? Will it copy the entire branch of the AST or will it only copy that specific node. Will I have to manually clone all the children and then patch the original copy to now point to the clones?

Are there already some utilities for copying parts of the AST?

---

_Comment by @MichaReiser on 2024-11-27 08:49_

> Fair enough, I will go the Transformer route and see how that ends up looking. I'm not familiar enough with the Clone trait to know what ast::Expr.clone() would do. How deep is the copy? Will it copy the entire branch of the AST or will it only copy that specific node. Will I have to manually clone all the children and then patch the original copy to now point to the clones?

It performs a deep clone of that expression. Many `Generator` based rules already use `clone` today, e.g. 

`Clone` should be all that you need. https://github.com/astral-sh/ruff/blob/fb94b71e63925dd3ad75066468384e9eccb102e3/crates/ruff_linter/src/rules/flake8_bugbear/rules/setattr_with_constant.rs#L49-L61

> Are there already some utilities for copying parts of the AST?



---

_Comment by @Daverball on 2024-11-27 09:22_

@MichaReiser One other flaw of `Generator` I have noticed is that `precedence` is completely private, I feel like there should at least be a public version of `unparse_expr` in addition to `expr` where you can pass in the current `precedence` so the unparsed subexpression doesn't end up with either too few or too many parentheses.

I'm not sure which way defaulting to `0` goes,  but it looks like it is the lowest precedence, which would potentially produce redundant parentheses around the given expression if I'm not mistaken, rather than missing required ones.

Missing required parentheses would be a bug magnet, that could change the semantics of generated expressions, but generating redundant parentheses seems kind of bad too.

It doesn't seem possible to provide a higher level entry point, where we automatically calculate the precedence based on the parent node, since you would need to know which of the attributes on the parent node the expression is attached to, since typically not all of them share the same precedence. So letting people optionally specify the precedence seems like the next best thing.

---

_Comment by @MichaReiser on 2024-11-27 09:32_

I don't mind adding `unparse_expr_with_precedence`, although I would prefer to make `Precedence` an `enum` first :laughing: 

---

_Closed by @Daverball on 2024-11-27 13:49_

---
