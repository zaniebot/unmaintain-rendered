```yaml
number: 14978
title: "[`pylint`] Preserve original value format (`PLR6104`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: PLR6104
created_at: 2024-12-15T01:07:38Z
updated_at: 2024-12-19T12:58:14Z
url: https://github.com/astral-sh/ruff/pull/14978
synced_at: 2026-01-12T15:55:49Z
```

# [`pylint`] Preserve original value format (`PLR6104`)

---

_@InSyncWithFoo_

## Summary

Resolves #11672.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-15 01:13_

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

_Comment by @InSyncWithFoo on 2024-12-15 01:15_

The existing implementation is surprisingly complex. On top of that, it uses the generator to create the fix, but that's not strictly necessary. String slicing is much simpler and only requires an extra hardcoded `if` for the `Expr::Named` case.

---

_Label `fixes` added by @AlexWaygood on 2024-12-15 15:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:170 on 2024-12-15 15:58_

I'd prefer if we could keep the enum and the associated methods (and avoid creating a `String`). Removing the `enum` also results in many unnecessary changes that makes reviewing the PR harder.

---

_@MichaReiser requested changes on 2024-12-15 15:59_

Thanks. Could you revert your changes to `AugmentedOperator` (and, if there are any, other refactors that aren't strictly necessary for the fix). I'd prefer to keep the changes minimal to the fix or have separate PRs for refactor and fix.

---

_@InSyncWithFoo reviewed on 2024-12-15 16:43_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:170 on 2024-12-15 16:43_

I reworked the fix from scratch to minimize the changes. I remain unsatisfied with how it is implemented, however. Should I follow this up with a refactoring PR?

---

_Comment by @InSyncWithFoo on 2024-12-15 17:15_

I'd like to believe it isn't my fault that the `mdformat` check failed. A simple `uvx pre-commit run --all-files` commit should fix it, but I won't submit a PR as I'm not interested in finding the cause.

---

_Comment by @AlexWaygood on 2024-12-15 19:06_

> I'd like to believe it isn't my fault that the `mdformat` check failed. A simple `uvx pre-commit run --all-files` commit should fix it, but I won't submit a PR as I'm not interested in finding the cause.

I believe it's due to one of the mdformat plugins we use having released a new version. We don't have these pinned in our pre-commit config file; we probably should.

Two of the plugins we use cut new releases a couple of hours ago, one of them a major version bump:
- https://github.com/KyleKing/mdformat-mkdocs/commit/ce6710808d35248d1419f5db89cb6e7a3fc6fd3d
- https://github.com/KyleKing/mdformat-admon/commit/f6f2c231072bf5f55ce6b4237f4ee908ba44eed2

---

_Comment by @AlexWaygood on 2024-12-15 19:33_

I filed https://github.com/astral-sh/ruff/pull/14992 to fix the pre-commit issue.

---

_@AlexWaygood reviewed on 2024-12-15 19:45_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:164 on 2024-12-15 19:45_

the `should_be_parenthesized_when_standalone()` function is so short, I feel like the code is easier to understand if the condition is just inlined:

```suggestion
    // expressions using the walrus operator (`:=`) must be parenthesized
    // when they're used as standalone expressions:
    let new_value_expr = if right_operand.is_named_expr() {
        format!("({right_operand_expr})")
    } else {
        right_operand_expr.to_string()
    };
    let new_content = format!("{target_expr} {operator} {new_value_expr}");

    Edit::range_replacement(new_content, range)
}
```

---

_@AlexWaygood reviewed on 2024-12-15 19:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:164 on 2024-12-15 19:49_

This could also be done with just one allocation rather than two, but fixes aren't generally that performance-sensitive and it would make the code less readable.

---

_@MichaReiser reviewed on 2024-12-16 09:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/non_augmented_assignment.py`:75 on 2024-12-16 09:09_

Would you mind adding an example where the expression starts on a new line

```suggestion
test4 = test4 + ( 
		4 
		* 
		10
)
```

I worry that the new logic generates an incorrect fix because the expression range doesn't include the parentheses. 

---

_@InSyncWithFoo reviewed on 2024-12-16 12:23_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pylint/non_augmented_assignment.py`:75 on 2024-12-16 12:23_

Added and fixed.

---

_@MichaReiser reviewed on 2024-12-17 08:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:200 on 2024-12-17 08:33_

Can we use `parenthesized_range` instead of implementing our own, adhoc lexing here. I think it's okay if the fix preserves parentheses, even if they're unnecessary (they were unnecessary before)

---

_@MichaReiser reviewed on 2024-12-17 08:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 08:34_

This seems complicated and I'm not sure I follow why it's necessary. 

Could we change the logic to instead take the range from after the operator to the end of the statement instead of trying to trim the range? 

If so, I suggest using `SimpleTokenizer` to find the operator over implementing your own `Cursor` logic

---

_@InSyncWithFoo reviewed on 2024-12-17 11:16_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 11:16_

Taking the range from after the operator is precisely what I'm doing (well, whitespace trimming too, but still).

I tried converting this to use `SimpleTokenizer` instead, but it isn't a `DoubleEndedIterator`. `trim_left_operator` can easily be reimplemented as:

```rust
let range = TextRange::new(0.into(), half_expr.text_len());

let mut tokenizer = SimpleTokenizer::new(half_expr, range);
let mut op_seen = false;

while let Some(token) = tokenizer.next() {
    match token.kind {
        SimpleTokenKind::Whitespace
        | SimpleTokenKind::Continuation
        | SimpleTokenKind::Newline => {}
        _ if op_seen => {
            return &half_expr[token.start().into()..];
        }
        SimpleTokenKind::Plus
        | SimpleTokenKind::Ampersand
        | SimpleTokenKind::Vbar
        | SimpleTokenKind::Circumflex
        | SimpleTokenKind::DoubleSlash
        | SimpleTokenKind::Slash
        | SimpleTokenKind::LeftShift
        | SimpleTokenKind::Less
        | SimpleTokenKind::At
        | SimpleTokenKind::Percent
        | SimpleTokenKind::DoubleStar
        | SimpleTokenKind::Star
        | SimpleTokenKind::RightShift
        | SimpleTokenKind::Greater
        | SimpleTokenKind::Minus => {
            op_seen = true;
        }
        _ => {}
    }
}

unreachable!("half_expr does not contain an operator");
```

The same, however, could not be said for `trim_right_operator`.

---

_@MichaReiser reviewed on 2024-12-17 11:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 11:34_

Yeah, we have a `BackwardsTokenizer` but it's not entirely clear why we even need to trim the right side. Can't we just use the statement's end? 

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 11:39_

"Trim" is probably not the best word. Here's how the `trim` functions work:

```text
x = x + (1 * 2)
     ^^^^^^^^^^ half_expr
x = x + (1 * 2)
        ^^^^^^^ trim_left_operator(half_expr)
```

```text
x = 1000000 + x
    ^^^^^^^^^^ half_expr
x = 1000000 + x
    ^^^^^^^ trim_right_operator(half_expr)
```

---

_@InSyncWithFoo reviewed on 2024-12-17 11:39_

---

_@MichaReiser reviewed on 2024-12-17 11:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 11:44_

Oh I see. The name's are a bit misleading (or you mixed up the examples) considering that `trim_right_operator` trims the left operand? 

Either way, I think we can once use the range from after `=` up to right before the operator and everything after the operator to the statement's end

---

_@InSyncWithFoo reviewed on 2024-12-17 11:59_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:142 on 2024-12-17 11:59_

`trim_left` trims the operator on the left of the expression whereas `trim_right` trims the operator on the right. These names are indeed confusing. I should have used `trim_start_operator` and `trim_end_operator`.

Anyway, I removed both of them. What I really needed but didn't know of was `parenthesized_range`.

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-17 12:29_

---

_@MichaReiser reviewed on 2024-12-17 13:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/non_augmented_assignment.rs`:147 on 2024-12-17 13:55_

I think the parent here is incorrect. It should be the parent of `right_operand_ref` and that's the binary expression

---

_@MichaReiser approved on 2024-12-17 15:07_

Thanks

---

_Merged by @MichaReiser on 2024-12-17 15:07_

---

_Closed by @MichaReiser on 2024-12-17 15:07_

---

_Branch deleted on 2024-12-17 16:53_

---

_Label `bug` added by @dhruvmanila on 2024-12-19 12:58_

---
