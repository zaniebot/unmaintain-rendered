```yaml
number: 14641
title: "[`flake8-pyi`, `ruff`] Fix traversal of nested literals and unions (`PYI016`, `PYI051`, `PYI055`, `PYI062`, `RUF041`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - bug
  - linter
assignees: []
merged: true
base: main
head: nested-literals-unions
created_at: 2024-11-27T18:06:09Z
updated_at: 2024-11-28T18:08:34Z
url: https://github.com/astral-sh/ruff/pull/14641
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-pyi`, `ruff`] Fix traversal of nested literals and unions (`PYI016`, `PYI051`, `PYI055`, `PYI062`, `RUF041`)

---

_Pull request opened by @sbrugman on 2024-11-27 18:06_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In previous work on PYI rules we found that the current traversal misses nested literals and unions.
These are rare but valid. As mentioned before, correctly handling these cases might be relevant for auto-generated stubs.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added test cases in an earlier PR. Snapshots are updated and show nested literals and unions are now handled as expected.
No ecosystem changes.

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-27 18:06_

---

_Comment by @github-actions[bot] on 2024-11-27 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.pyi.snap`:144 on 2024-11-27 19:07_

hmm, this isn't an equivalent rewrite. `type[float, int]` isn't a valid type expression (it would need to be either `type[float | int]` or `type[Union[float, int]]`, and therefore `Union[type[float, int], type[complex]]` isn't a valid type expression, and therefore `Union[Union[Union[type[float, int], type[complex]]]]` isn't a valid type expression. The rewrite here turns it into something that _is_ valid.

I think it would be better if we refused the temptation to guess here and did not emit the diagnostic at all if `type` is subscripted with multiple arguments.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.pyi.snap`:165 on 2024-11-27 19:08_

same issue here: `type[float, int]` isn't valid in a type annotation, therefore the whole annotation is invalid, but the fix here turns it into something that's valid. That should be outside the scope of this rule.

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1536 on 2024-11-27 19:20_

I don't understand why we have to consider `current_expression_parent()` *and* `current_expression_grandparent()` for subscript-based unions, but we only have to consider `current_expression_parent()` for `|`-based unions. Does that mean that this function will recognise this as being a nested union:

```py
Union[Union[int, str]]
```

but not necessarily this?

```py
Union[int, str] | Union[bytes, list]
```

---

_@AlexWaygood reviewed on 2024-11-27 19:21_

Thank you!! Most of the snapshot changes look great, but I spotted a couple of issues and I'm not sure I fully understand the changes you're making to the methods on `SemanticModel`

---

_Label `linter` added by @AlexWaygood on 2024-11-27 19:41_

---

_Comment by @sbrugman on 2024-11-28 10:36_

Thanks Alex! I'll pick this up tomorrow.

---

_@sbrugman reviewed on 2024-11-28 11:01_

---

_Review comment by @sbrugman on `crates/ruff_python_semantic/src/model.rs`:1536 on 2024-11-28 11:01_

I think this is because the PEP604-style unions are always binary operations, but I need to double check.


---

_@sbrugman reviewed on 2024-11-28 11:11_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.pyi.snap`:144 on 2024-11-28 11:11_

Good catch. I see this behaviour was already in place before, where we fixed an invalid type expression to a valid type expression:

```diff
-x: Union[Union[type[float, int], type[complex]]]
+x: Union[type[Union[float, int, complex]]]
```

In this PR we just handle the nesting better:

```diff
-x: Union[Union[type[float, int], type[complex]]]
+x: type[Union[float, int, complex]]
```

But the premise of the fix is still wrong, and needs to be fixed in PYI055.


---

_@sbrugman reviewed on 2024-11-28 13:21_

---

_Review comment by @sbrugman on `crates/ruff_python_semantic/src/model.rs`:1536 on 2024-11-28 13:21_

This is indeed the case. I've added an additional test case to confirm.

---

_@sbrugman reviewed on 2024-11-28 13:22_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.pyi.snap`:144 on 2024-11-28 13:22_

I'll address this in a separate PR so that the change gets its own entry in the changelog.

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/model.rs`:1536 on 2024-11-28 13:51_

Okay, I think I now understand why we must consider both the `current_expression_grandparent()` and the `current_expression_parent()` for subscript unions. Please tell me if I misunderstand anything here.

It's because there's a difference in AST structure for these two nested unions:

```py
A = Union[Union[int]]
B = Union[Union[int], bytes]
```

For union `A`, there, the AST structure looks like this (simplifying a little; this isn't valid Rust, obviously):

```rs
Subscript {
    value: Name("Union")
    slice: Subscript {
        value: Name("Union")
        slice: Name("int")
    }
}
```

But for union `B`, the AST structure looks like this:

```rs
Subscript {
    value: Name("Union")
    slice: Tuple([
        Subscript {
            value: Name("Union")
            slice: Name("int")
        },
        Name("bytes")
    ])
}
```

So from the perspective of the inner union for `A`, the outer union is the `current_expression_parent` (there's no intermediate `Tuple` expression because there's only one element in the subscript slice). But from the perpespective of the inner union for `B`, the outer union is the `current_expression_grandparent` (there's an intermediate `Tuple` expression because there's more than one element in the subscript slice).

Is that correct? If so, please could you expand on the comments you have here, as it took me quite a while to understand all this :-)

I also think you could make the implementation more efficient and accurate (accurate in that `current_expression_parent` must be a `Tuple` as well as `current_expression_grandparent` being a `Union` subscript in order for it to be a nested union). Something like this?

```rs
    /// Return `true` if the model is in a nested union expression (e.g., the inner `Union` in
    /// `Union[Union[int, str], float]`).
    pub fn in_nested_union(&self) -> bool {
        let mut parent_expressions = self.current_expressions().skip(1);

        match parent_expressions.next() {
            Some(Expr::Subscript(parent)) => self.match_typing_expr(&parent.value, "Union"),
            Some(Expr::Tuple(_)) => parent_expressions
                .next()
                .and_then(Expr::as_subscript_expr)
                .is_some_and(|grandparent| self.match_typing_expr(&grandparent.value, "Union")),
            Some(Expr::BinOp(bin_op)) => bin_op.op.is_bit_or(),
            _ => false,
        }
```

---

_@AlexWaygood reviewed on 2024-11-28 13:51_

---

_@AlexWaygood reviewed on 2024-11-28 13:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.pyi.snap`:144 on 2024-11-28 13:54_

Makes sense

---

_@sbrugman reviewed on 2024-11-28 15:23_

---

_Review comment by @sbrugman on `crates/ruff_python_semantic/src/model.rs`:1536 on 2024-11-28 15:23_

100%, this is much cleaner 👍 I'll expand this with comments and rewrite `in_nested_literal` in a similar fashion.

---

_@AlexWaygood approved on 2024-11-28 17:56_

Thank you!!

---

_Label `bug` added by @AlexWaygood on 2024-11-28 17:57_

---

_Renamed from "Fix traversal of nested literals and unions" to "[`flake8-pyi`, `ruff`] Fix traversal of nested literals and unions (`PYI016`, `PYI051`, `PYI055`, `PYI062`, `RUF041`)" by @AlexWaygood on 2024-11-28 18:07_

---

_Merged by @AlexWaygood on 2024-11-28 18:07_

---

_Closed by @AlexWaygood on 2024-11-28 18:07_

---

_Branch deleted on 2024-11-28 18:08_

---
