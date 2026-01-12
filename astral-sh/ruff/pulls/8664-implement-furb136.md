```yaml
number: 8664
title: Implement FURB136
type: pull_request
state: merged
author: siiptuo
labels:
  - rule
assignees: []
merged: true
base: main
head: FURB136
created_at: 2023-11-13T20:48:17Z
updated_at: 2023-11-15T18:16:26Z
url: https://github.com/astral-sh/ruff/pull/8664
synced_at: 2026-01-12T15:55:26Z
```

# Implement FURB136

---

_@siiptuo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implements [FURB136](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb136-use-min-max) that checks for `if` expressions that can be replaced with `min()` or `max()` calls. See issue #1348 for more information.

This implementation diverges from Refurb's original implementation by retaining the order of equal values. For example, Refurb suggest that the following expressions:

```python
highest_score1 = score1 if score1 > score2 else score2
highest_score2 = score1 if score1 >= score2 else score2
```

should be to rewritten as:

```python
highest_score1 = max(score1, score2)
highest_score2 = max(score1, score2)
```

whereas this implementation provides more correct alternatives:

```python
highest_score1 = max(score2, score1)
highest_score2 = max(score1, score2)
```

## Test Plan

<!-- How was it tested? -->

Unit test checks all eight possibilities.

---

_Label `rule` added by @charliermarsh on 2023-11-13 20:55_

---

_Comment by @github-actions[bot] on 2023-11-13 21:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/946e1e0c4e078aec6d76c708d578af053fdd62b6/airflow/providers/apache/kafka/operators/consume.py#L158'>airflow/providers/apache/kafka/operators/consume.py:158:30:</a> FURB136 [*] Replace `if` expression with `min(messages_left, self.max_batch_size)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB136 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh reviewed on 2023-11-13 21:06_

Nice, thank you, this looks great! I may make some small tweaks to better conform to some of our idioms but shouldn't require any major changes.

---

_@charliermarsh reviewed on 2023-11-13 21:07_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1305 on 2023-11-13 21:07_

Can you instead pass in the `ast::ExprIfExp` here? That's what we prefer for new rules. So e.g. on line 1284, you'd change to:

```rust
Expr::IfExp(if_exp @ ast::ExprIfExp {
    test,
    body,
    orelse,
    range: _,
}) => {
    ...
}
```

And then here, you'd pass in `if_exp` instead of the destructured fields. That way, callers can't pass in the "wrong" values.

---

_@siiptuo reviewed on 2023-11-14 07:37_

---

_Review comment by @siiptuo on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:1305 on 2023-11-14 07:37_

Good point!

---

_@charliermarsh approved on 2023-11-15 17:54_

Thanks!

---

_Merged by @charliermarsh on 2023-11-15 18:10_

---

_Closed by @charliermarsh on 2023-11-15 18:10_

---
