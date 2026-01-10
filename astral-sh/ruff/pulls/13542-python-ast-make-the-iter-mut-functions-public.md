```yaml
number: 13542
title: "[python_ast] Make the iter_mut functions public"
type: pull_request
state: merged
author: ndmitchell
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: ndmitchell-pub-iter-mut
created_at: 2024-09-27T19:06:26Z
updated_at: 2024-10-21T18:50:32Z
url: https://github.com/astral-sh/ruff/pull/13542
synced_at: 2026-01-10T20:59:36Z
```

# [python_ast] Make the iter_mut functions public

---

_Pull request opened by @ndmitchell on 2024-09-27 19:06_

## Summary

I needed to use these, so expose them. I am manipulating an AST to mutate it, and can't get at the inner strings here.

## Test Plan

CI

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-27 19:12_

The fact that this method isn't public seems intentional to me because mutating can result in an inconsistent to_str result.

---

_@MichaReiser reviewed on 2024-09-27 19:12_

---

_Comment by @github-actions[bot] on 2024-09-27 19:25_

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

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-27 20:31_

For my particular usage, I actually only want to mutate the ranges. Thoughts on how to do that in a safe way?

---

_@ndmitchell reviewed on 2024-09-27 20:31_

---

_@ndmitchell reviewed on 2024-09-28 10:29_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-28 10:29_

In fact, if I had Parser::new_starts_at exposed that would suit my needs even better. I have a string, but I know it starts halfway through a file, and what to parse that.

---

_@ndmitchell reviewed on 2024-09-28 10:49_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-28 10:49_

And I guess this only applies to the StringLiteral one - since the others don't have a cached to_str, they could be made public?

---

_@MichaReiser reviewed on 2024-09-28 11:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-28 11:11_

The parser exposes a parse_expression_range method. I rather not expose the parser struct 

---

_@ndmitchell reviewed on 2024-09-28 14:29_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-28 14:29_

I need to iterate mutably over FString's for other purposes. Would you be OK exposing the iter_mut for FString and BytesLiteral? Or even just FString? (On the basis BytesLiteral could one day get a to_str, but FString never will)

The parse_expression_range gets a string and parses a subset of it. I instead want to parse the full string, but with an implicit offset. Currently I'm faking that by creating a string of 1000+ spaces, then the expression I care about, and using parse_expression_range. That's pretty inefficient.

---

_@MichaReiser reviewed on 2024-09-28 15:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-28 15:52_

Do you have some code that you can share. I don't understand the use case and I rather not expose iter_mut because it breaks the type's encapsulation 

Have you considered recreating the entire ExprStringLiteral and cloning the parts? 

For the offset. The main use case for the offsets is so that you can slice into the string but that's not what you want. Have you considered offsetting the ranges e.g when printing the errors or what's the use case for the offsets?

---

_@ndmitchell reviewed on 2024-09-29 19:43_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-29 19:43_

I am writing a Python type checker. I've already talked to some Astral folks (Carl - who we should chat with again). Two things:

* In the Python typing spec, you can have strings in place of types. E.g. `def f(x: "list[int]"):`. So you now have a string literal `list[int]` and want to turn it into a type, which is just an expression, so want to call `parse_expression`. But if you can't find `list` etc, you want to report exactly where you couldn't find list. You could of course shift all the ranges (but not unless you expose iter_mut) or shift them for a subset of expressions when printing (but that's super error prone - now you need to wrap every expression with its tracking offset). The easiest thing is to just parse with an implicit offset. Currently we do that by creating a really long string of spaces and using parse_expression_range. You can't just parse from the original string, as there might be things like escapes or multiple literals (which you have to pad out with spaces to get the right positions - which isn't too hard to do).

* Part of our type checker goes walking through expressions to substitute them out with other expressions, to do some book keeping and make some expressions point at other places. Concretely we have `fn expr_visit_mut<'a>(x: &'a mut Expr, f: impl FnMut(&'a mut Expr))` which calls the callback on every one-step recursive function. That's a super handy generic primitive to have. But alas, FString (and only FString) doesn't expose enough pieces to get at the mutable iterators inside. Having `iter_mut` on FString would be valuable and allow such a function. We actually have `expr_visit_mut`, `expr_visit`, `stmt_visit_mut`, `stmt_visit`, `stmt_expr_visit_mut`, `stmt_expr_visit` (the last two finding the expressions inside a statement). It would be wonderful if all those were provided on the AST for free. We use them for loads of things.

---

_@ndmitchell reviewed on 2024-09-29 20:43_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-29 20:43_

(in all cases we'd be delighted to supply them as a PR, once we figure out the design)

---

_@dhruvmanila reviewed on 2024-09-30 13:22_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-30 13:22_

For (1), we do use `parse_expression_range`, you might be interested in going through https://github.com/astral-sh/ruff/blob/5118166d21784e7c78e38ea42919ba50bb2a5142/crates/ruff_python_parser/src/typing.rs which parses string type annotations. Ruff still has a bug for complex type annotations (like implicitly concatenated strings) https://github.com/astral-sh/ruff/issues/10586.

For (2), we have the [`Transformer` trait](https://github.com/astral-sh/ruff/blob/5118166d21784e7c78e38ea42919ba50bb2a5142/crates/ruff_python_ast/src/visitor/transformer.rs) which will visit the nodes to allow mutation.

Is this useful?

---

_@MichaReiser reviewed on 2024-09-30 13:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-09-30 13:52_

We are also looking into how to solve 1 for our type checker

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-10-01 12:52_

@dhruvmanila - yep, we have the identical problem, but haven't added the "simple literal" special case. We probably should. But then still end up with the same issue. If you had the parse_expression_at, then the special case becomes non-interesting and you can simplify the code.

The Transformer trait is super interesting. It's a shame that the self isn't mut - that makes accumulating in the visitor much harder. It's a shame there is no lifetime constraint, e.g. if you are walking an `&'a Stmt`, you have the guarantee that all the statements you bump into have the same lifetime `'a` (I note SourceOrderVisitor has this). Can't you use this to screw with the literals in a StringLiteral, thus breaking the `to_str` cache? With those adjustments, I think we'd implement our visitors on top of your Transformer trait.

---

_@ndmitchell reviewed on 2024-10-01 12:52_

---

_@ndmitchell reviewed on 2024-10-01 13:15_

---

_Review comment by @ndmitchell on `crates/ruff_python_ast/src/nodes.rs`:1752 on 2024-10-01 13:15_

Alternatively, if you added `parse_expression_in_string_literal(x: &StringLiteral) -> Expr` then that would be _exactly_ the primitive we both need, we could delete the slightly dubious `parse_expression_range` (is there another good reason to have this?) and you could do very clever things to deal with things like escapes only once.

---

_Comment by @MichaReiser on 2024-10-07 07:31_

I would like to wait with this PR until we decided on how to solve https://github.com/astral-sh/ruff/discussions/13499

---

_@MichaReiser approved on 2024-10-19 19:03_

This is probably fine. The AST is really just a data object and provides little to no abstraction. It's certainly a footgun but it's already possible to mutate the tree in place in the transformer.

We may decide to remove those accessors in the future but for now, this seems fine.

---

_Label `internal` added by @MichaReiser on 2024-10-19 19:03_

---

_Label `parser` added by @MichaReiser on 2024-10-19 19:03_

---

_Merged by @MichaReiser on 2024-10-19 19:04_

---

_Closed by @MichaReiser on 2024-10-19 19:04_

---

_Branch deleted on 2024-10-21 18:50_

---
