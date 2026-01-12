```yaml
number: 16564
title: "Fix `useless-try-except (TRY203)` false negative"
type: pull_request
state: closed
author: harupy
labels: []
assignees: []
draft: true
base: main
head: fix-try-203
created_at: 2025-03-08T08:56:35Z
updated_at: 2025-06-21T09:02:06Z
url: https://github.com/astral-sh/ruff/pull/16564
synced_at: 2026-01-12T15:55:55Z
```

# Fix `useless-try-except (TRY203)` false negative

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #16561.

## Test Plan

<!-- How was it tested? -->

New test cases

---

_Comment by @github-actions[bot] on 2025-03-08 09:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/ccb6ffa74dfa530a078214c10504497577793472/tests/projects/utils.py#L79'>tests/projects/utils.py:79:9:</a> TRY203 Remove exception handler; error is immediately re-raised
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY203 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/ccb6ffa74dfa530a078214c10504497577793472/tests/projects/utils.py#L79'>tests/projects/utils.py:79:9:</a> TRY203 Remove exception handler; error is immediately re-raised
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TRY203 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Closed by @harupy on 2025-06-21 09:02_

---
