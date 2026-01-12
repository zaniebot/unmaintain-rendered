```yaml
number: 18866
title: "[`perflint`] Fix false negative in `PERF401`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-PERF403
created_at: 2025-06-22T20:57:34Z
updated_at: 2025-06-25T14:00:13Z
url: https://github.com/astral-sh/ruff/pull/18866
synced_at: 2026-01-12T15:56:26Z
```

# [`perflint`] Fix false negative in `PERF401`

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR also fixes a false negative for the following case:
```python
for o,(x,)in():
    v[x,]=o
``` 
The code didn't consider the case of inner tuples in the `for` target.

And finally, the autofix would create a invalid syntax if the subscript expression had a tuple in the slice.

Fixes #18859
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-22 21:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/73db25d585a12e587beffef83449dbdd5d16d0f6/pandas/tests/arrays/sparse/test_accessor.py#L247'>pandas/tests/arrays/sparse/test_accessor.py:247:30:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
+ <a href='https://github.com/pandas-dev/pandas/blob/73db25d585a12e587beffef83449dbdd5d16d0f6/pandas/tests/arrays/sparse/test_accessor.py#L96'>pandas/tests/arrays/sparse/test_accessor.py:96:50:</a> RUF043 Pattern passed to `match=` contains metacharacters but is neither escaped nor raw
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF043 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:177 on 2025-06-23 09:18_

I'm not sure this fix is correct. Shouldn't we also skip the rule if any nested name is used after the loop? E.g. when the name is inside a nested tuple or similar. 

That's why I think we either need to apply the check recursively or bail in more cases.

---

_@MichaReiser reviewed on 2025-06-23 09:18_

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-23 21:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:152 on 2025-06-24 07:55_

Nit: I would move this all into `has_post_loop_references`:

```rust
fn has_post_loop_references(checker: &Checker, expr: &Expr, loop_end: TextSize) -> bool {
    match expr {
        Expr::Tuple(ast::ExprTuple { elts, .. }) => elts
            .iter()
            .any(|expr| has_post_loop_references(checker, expr, loop_end)),
        Expr::Name(name) => {
            let target_binding = checker
                .semantic()
                .bindings
                .iter()
                .find(|binding| name.range() == binding.range)
                .expect("for-loop target binding must exist");

            target_binding
                .references()
                .map(|reference| checker.semantic().reference(reference))
                .any(|other_reference| other_reference.start() > loop_end)
        }
        _ => false,
    }
}
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:175 on 2025-06-24 07:56_

We can now move this check above the match statement because `has_post_loop_references` takes any `Expr`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:461 on 2025-06-24 07:56_

I think this method is still incorrect because it doesn't traverse into arbitrary expressions. Instead, we should use `any_over_expr` and only check `ExprName` in load contexts.

For example:

```py
a = []
b = [10]
x = 0
 
for (a, b[x]) in [(1, 2)]: pass

x = 10
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview__PERF403_PERF403.py.snap`:360 on 2025-06-24 08:04_

This PR does seem to change the rules that get reported because this wasn't a violation before (when building in release build)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:359 on 2025-06-24 08:11_

Instead of bailing here, should the rule still be triggered but without providing a fix?

---

_@MichaReiser reviewed on 2025-06-24 08:11_

I've a few more questions and a refactor suggestion

---

_Label `rule` added by @MichaReiser on 2025-06-24 08:12_

---

_Renamed from "[`perflint`] Fix panic in `PERF401`" to "[`perflint`] Fix false negative in `PERF401`" by @MichaReiser on 2025-06-24 08:12_

---

_@LaBatata101 reviewed on 2025-06-24 14:22_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/perflint/rules/manual_dict_comprehension.rs`:359 on 2025-06-24 14:22_

It only bails if the slice has more than one expression. I think the rule doesn't apply for slices with more than one expression.

---

_@LaBatata101 reviewed on 2025-06-24 14:38_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview__PERF403_PERF403.py.snap`:360 on 2025-06-24 14:38_

Are you talking about the ecosystem changes? If so, It doesn't make sense for `RUF043` to be triggering here, it doesn't make use of code changed in this PR.

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-24 14:39_

---

_@MichaReiser reviewed on 2025-06-24 14:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview__PERF403_PERF403.py.snap`:360 on 2025-06-24 14:57_

No, I'm saying that this specific example wasn't a violation on main, see https://play.ruff.rs/7408fd27-bd76-4559-a83c-934959b8e498. That means that this PR does more than just fix a panic, it also extends the scope of the rule to support nested tuples

---

_@LaBatata101 reviewed on 2025-06-24 15:26_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview__PERF403_PERF403.py.snap`:360 on 2025-06-24 15:26_

Oh right,  now I see what you meant.

---

_Label `bug` added by @MichaReiser on 2025-06-25 08:44_

---

_Comment by @MichaReiser on 2025-06-25 08:44_

Thank you

---

_Merged by @MichaReiser on 2025-06-25 08:44_

---

_Closed by @MichaReiser on 2025-06-25 08:44_

---

_Branch deleted on 2025-06-25 14:00_

---
