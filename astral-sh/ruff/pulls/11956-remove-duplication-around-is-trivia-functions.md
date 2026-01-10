```yaml
number: 11956
title: "Remove duplication around `is_trivia` functions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/trivia
created_at: 2024-06-21T05:09:15Z
updated_at: 2024-06-21T10:18:18Z
url: https://github.com/astral-sh/ruff/pull/11956
synced_at: 2026-01-10T21:56:00Z
```

# Remove duplication around `is_trivia` functions

---

_Pull request opened by @dhruvmanila on 2024-06-21 05:09_

## Summary

This PR removes the duplication around `is_trivia` functions.

There are two of them in the codebase:
1. In `pycodestyle`, it's for newline, indent, dedent, non-logical newline and comment
2. In the parser, it's for non-logical newline and comment

The `TokenKind::is_trivia` method used (1) but that's not correct in that context. So, this PR introduces a new `is_non_logical_token` helper method for the `pycodestyle` crate and updates the `TokenKind::is_trivia` implementation with (2).

This also means we can remove `Token::is_trivia` method and the standalone `token_source::is_trivia` function and use the one on `TokenKind`.

## Test Plan

`cargo insta test`

---

_Label `internal` added by @dhruvmanila on 2024-06-21 05:09_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-21 05:09_

---

_Comment by @github-actions[bot] on 2024-06-21 05:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5a8c3d5efd45b197601a88d7c27569b6c466a48b/src/plasmapy/diagnostics/langmuir.py#L1396'>src/plasmapy/diagnostics/langmuir.py:1396:23:</a> NPY201 `np.trapz` will be removed in NumPy 2.0. Use `numpy.trapezoid` on NumPy 2.0, or ignore this warning on earlier versions.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY201 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/helpers.rs`:20 on 2024-06-21 06:01_

Nit: `token.is_trivia() || matches!(token, TokenKind::Newline | TokenKind::Indent | TokenKind::Dedent)

---

_@MichaReiser approved on 2024-06-21 06:01_

---

_Merged by @dhruvmanila on 2024-06-21 10:02_

---

_Closed by @dhruvmanila on 2024-06-21 10:02_

---

_Branch deleted on 2024-06-21 10:02_

---
