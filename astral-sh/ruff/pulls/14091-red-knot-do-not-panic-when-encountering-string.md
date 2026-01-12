```yaml
number: 14091
title: "[red-knot] Do not panic when encountering string annotations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/no-panic-for-stringified-annotations
created_at: 2024-11-04T13:12:37Z
updated_at: 2024-11-04T20:29:38Z
url: https://github.com/astral-sh/ruff/pull/14091
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Do not panic when encountering string annotations

---

_@sharkdp_

## Summary

Encountered this while running red-knot benchmarks on the `black` codebase.

Fixes two of the issues in #13478.

## Test Plan

Added a regression test.

---

_Label `red-knot` added by @sharkdp on 2024-11-04 13:12_

---

_Review requested from @carljm by @sharkdp on 2024-11-04 13:12_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-04 13:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-04 13:12_

---

_@MichaReiser reviewed on 2024-11-04 13:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:201 on 2024-11-04 13:16_

I'm a bit concerned that will forget to remove this after adding support for stringified type annotations. 

Should we instead insert the type TODO here

https://github.com/astral-sh/ruff/blob/985f21168f516c42a269defca0e6151c87835076/crates/red_knot_python_semantic/src/types/infer.rs#L3748-L3749

---

_Comment by @github-actions[bot] on 2024-11-04 13:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-11-04 13:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:201 on 2024-11-04 13:30_

> Should we instead insert the type TODO here
> 
> [`985f211`/crates/red_knot_python_semantic/src/types/infer.rs#L3748-L3749](https://github.com/astral-sh/ruff/blob/985f21168f516c42a269defca0e6151c87835076/crates/red_knot_python_semantic/src/types/infer.rs#L3748-L3749)

![image](https://github.com/user-attachments/assets/07c3dfd8-c924-48c0-ba80-d70f92d4d000)

😶

---

_@sharkdp reviewed on 2024-11-04 13:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:201 on 2024-11-04 13:34_

> Should we instead insert the type TODO here

We already have this branch:
```rs
// TODO: parse the expression and check whether it is a string annotation.
// https://typing.readthedocs.io/en/latest/spec/annotations.html#string-annotations
ast::Expr::StringLiteral(_literal) => Type::Todo,
```

It does not prevent the panic.

But I think I know what's wrong. Will push a better fix (hopefully).

---

_@sharkdp reviewed on 2024-11-04 13:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3661 on 2024-11-04 13:38_

If we do not add the expression type into `self.types.expressions` here (like we do for the other branches), it will later lead to a panic here:

https://github.com/astral-sh/ruff/blob/df45a0e3f966dd24feb0fece101dfa469ff24d1b/crates/red_knot_python_semantic/src/types.rs#L199

---

_@MichaReiser approved on 2024-11-04 13:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string_annotations.md`:1 on 2024-11-04 13:49_

nit: If all we're asserting is "make sure we don't panic", then an mdtest is arguably overkill here: this could be better as a snippet added to the [corpus directory](https://github.com/astral-sh/ruff/tree/main/crates/red_knot_workspace/resources/test/corpus) (we have a test that runs over all the snippets in that directory and checks that we can at least run red-knot over them without panicking)

---

_@AlexWaygood approved on 2024-11-04 13:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/string_annotations.md`:1 on 2024-11-04 13:53_

I thought we would eventually have a Markdown-based test for string annotations anyway, and this test (with a non-`@Todo`-assertion) could very well be a basic initial test. I agree, so far it does little except document the fact that we don't understand string annotations yet.

But I'm happy to turn it into a snippet, if you prefer that.

---

_@sharkdp reviewed on 2024-11-04 13:53_

---

_@AlexWaygood reviewed on 2024-11-04 14:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/string_annotations.md`:1 on 2024-11-04 14:00_

Yeah, fair enough. I suppose the corpus directory becomes less and less useful as we expand our ambitions beyond "do not panic when checking Python" to "infer the correct types consistently everywhere when checking Python" 😆

---

_Merged by @sharkdp on 2024-11-04 14:06_

---

_Closed by @sharkdp on 2024-11-04 14:06_

---

_Branch deleted on 2024-11-04 14:06_

---

_@carljm reviewed on 2024-11-04 20:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3664 on 2024-11-04 20:21_

I suspect we have the same bug here, for starred expressions in an annotation.

In general the contract is supposed to be that `infer_expression` takes care of storing the expression type, so we don't have to do it separately for every kind of expression.

For annotations, the tricky thing is the split between annotation expressions (what is valid in an annotation) and type expressions (which is a subset of annotation expressions). Currently `infer_type_expression` stores the type, but `infer_annotation_expression` does not, because most of the time it just defers to `infer_type_expression`, and we don't want to double-store. But we should come up with some general way to ensure annotation expression types are always stored, without having to do it separately in each branch that we add here.

---

_@sharkdp reviewed on 2024-11-04 20:27_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3664 on 2024-11-04 20:27_

Oh, I *did* look at `infer_starred_expression` and saw that it uses `infer_expression` (which stores the expression type), but that's probably not enough… I'll look into it!

---

_@carljm reviewed on 2024-11-04 20:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3664 on 2024-11-04 20:29_

It uses `infer_expression` on a sub-part of the starred expression (the `value`), but never on the starred expression itself. This is the usual pattern for `infer_*` methods for particular node types, since the assumption is they are called from `infer_expression` which will store the type they return.

---
