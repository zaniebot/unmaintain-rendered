```yaml
number: 13909
title: "Fix: Recover boolean test flag after visiting subexpressions"
type: pull_request
state: merged
author: Lokejoke
labels:
  - internal
assignees: []
merged: true
base: main
head: bool-test-scope-fix
created_at: 2024-10-24T12:40:35Z
updated_at: 2024-11-05T19:55:49Z
url: https://github.com/astral-sh/ruff/pull/13909
synced_at: 2026-01-10T20:50:57Z
```

# Fix: Recover boolean test flag after visiting subexpressions

---

_Pull request opened by @Lokejoke on 2024-10-24 12:40_

<!--
Thank you for contributing to Ruff! To assist with the review, please ensure:
- The pull request includes a summary of the change (below).
- The title is descriptive of the change.
- Relevant issues are referenced where applicable.
-->

## Summary

<!-- What is the purpose of the change? What does it do, and why? -->

This PR corrects the logic for handling the boolean test flag during expression analysis.

Since the flag can be modified during subexpression traversal, it must be restored before analyzing the full expression.

Additionally, this change allows the `in_bool_test()` method in `crates/ruff_python_semantic/src/model.rs` to be used outside of boolean operations, such as in conditional tests within `Stmt::If`, `Stmt::While`, `Stmt::Assert`, and others.


---

_Comment by @github-actions[bot] on 2024-10-24 12:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-10-24 12:57_

Thanks. Would you mind adding a test demonstrating that restoring the flag is necessary?

---

_Comment by @Lokejoke on 2024-10-25 12:04_

> Thanks. Would you mind adding a test demonstrating that restoring the flag is necessary?

@MichaReiser Could you direct me to where this test would fit? 


---

_Comment by @MichaReiser on 2024-10-25 12:10_

We unfortunately don't have any semantic model tests. So the best you can do is to extend the tests of a rule that uses the flag. One such rule is `RUF019` and the tests are defined here https://github.com/astral-sh/ruff/blob/e37bde458e9928f1997cba249471459d31136aee/crates/ruff_linter/src/rules/ruff/mod.rs#L46 (in `RUF019.py`)

---

_@MichaReiser requested changes on 2024-10-29 17:17_

Could you explain me why restoring the flags here is necessary. 

To me, it doesn't seem necessary because restoring the `semantic.flags` here means that the `BOOLEAN_TEST` flag won't be set for the `analyze::expression` phase. 

We do reset the `semantic.flags on line 1480 so that it is correctly restored for its ancestor and sibling expressions



---

_Comment by @Lokejoke on 2024-11-01 18:21_

Take for example this simple code:

``` python
if not f(a):
    pass
```

Currently, when visiting the expression f(a), it loses its BOOLEAN_TEST flag. This happens because it doesn’t match the conditions for `BoolOp` or `Not`, following the logic noted in the comment (lines 1032-1033):

>// If we're in a boolean test (e.g., the `test` of a `Stmt::If`), but now within a
>// subexpression (e.g., `a` in `f(a)`), then we're no longer in a boolean test.

But the `f(a)` it self should be in bool test so the flag should be recovered before analysis.

---
# Example with Current vs. Proposed Behavior
To see the difference, here’s a comparison using various test cases:

``` python
if not a(b):
    pass

assert c(d)

x = e(f) or g(h)

while i == j:
    pass
```

1. Current BOOLEAN_TEST Flag Behavior (restoring the flag after analysis):
Expressions with BOOLEAN_TEST flag:
* `not a(b)`

2. Proposed BOOLEAN_TEST Flag Behavior (restoring the flag before analysis):
Expressions with BOOLEAN_TEST flag:
* `not a(b)`
* `a(b)`
* `c(d)`
* `i == j`

---

# Testing the Change
Testing this change with current rules is difficult, as they cover only Expr::BoolOp. However, implementing rule `use-implicit-booleaness-not-len` / `C1802` would reveal the issue, as it checks for cases where `BOOLEAN_TEST` is essential to detect rule violations (in process).





---

_Comment by @dhruvmanila on 2024-11-04 03:45_

> However, implementing rule `use-implicit-booleaness-not-len` / `C1802` would reveal the issue, as it checks for cases where `BOOLEAN_TEST` is essential to detect rule violations (in process).

Can you expand on why do you think it's an issue for this specific rule? I think that rule would need to be implemented at the `ExprCall` level for which the `BOOLEAN_TEST` flag should be test. It's only when the checker is visiting a sub-expression like an argument to the function call when the flag is being reset.

---

_Comment by @Lokejoke on 2024-11-04 11:11_

I intended to illustrate with the rule example that the `BOOLEAN_TEST` flag is not limited to `BoolOp` and `Not` expressions, even though it isn’t used elsewhere in the project.

I agree with your interpretation of how the `BOOLEAN_TEST` flag should function, but the current implementation doesn’t reflect this expected behavior.

1. Store the initial flags.
2. Remove the `BOOLEAN_TEST` flag for any expressions that are not `BoolOp` or `Not`.
3. Visit subexpressions.
4. Analyze the expression.
5. Restore the original flags.

`BOOLEAN_TEST` flag should be restored between steps 3 and 4.





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1471 on 2024-11-04 11:21_

Thanks for the explanation. The change now makes sense to me. However, I do think that this problem isn't specific to `BOOLEAN_TEST` but applies to all flags. 
The flags should be restored when visiting the expression itself. They're only relevant when visiting the sub-expressions. 

That's why I suggest that we change the code to:

```rust
        // Step 4: Analysis
        match expr {
            Expr::StringLiteral(string_literal) => {
                analyze::string_like(string_literal.into(), self);
            }
            Expr::BytesLiteral(bytes_literal) => analyze::string_like(bytes_literal.into(), self),
            Expr::FString(f_string) => analyze::string_like(f_string.into(), self),
            _ => {}
        }
        
        self.semantic.flags = flags_snapshot;
        analyze::expression(expr, self);
```

---

_@MichaReiser reviewed on 2024-11-04 11:21_

---

_Label `internal` added by @MichaReiser on 2024-11-04 11:21_

---

_@Lokejoke reviewed on 2024-11-04 11:43_

---

_Review comment by @Lokejoke on `crates/ruff_linter/src/checkers/ast/mod.rs`:1471 on 2024-11-04 11:43_

I'm not sure about this. Could there be cases where a subexpression changes the semantics of its parent expression?
If so, this approach might not be correct.

---

_@MichaReiser reviewed on 2024-11-04 11:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1471 on 2024-11-04 11:48_

I skimmed through the `SemanticFlags` and I didn't spot any expression level flag that would change the flags for the parent or a sibling.

---

_Comment by @MichaReiser on 2024-11-05 19:55_

Thanks and nice find!

---

_Merged by @MichaReiser on 2024-11-05 19:55_

---

_Closed by @MichaReiser on 2024-11-05 19:55_

---
