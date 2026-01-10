```yaml
number: 20245
title: "[`RUF102`] Respect rule redirects in invalid rule code detection"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - fixes
  - suppression
assignees: []
merged: true
base: main
head: ruf102-respect-rule-redirects
created_at: 2025-09-04T18:00:25Z
updated_at: 2025-09-10T21:27:53Z
url: https://github.com/astral-sh/ruff/pull/20245
synced_at: 2026-01-10T17:46:21Z
```

# [`RUF102`] Respect rule redirects in invalid rule code detection

---

_Pull request opened by @TaKO8Ki on 2025-09-04 18:00_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20235

• Fix `RUF102` to properly handle rule redirects when validating noqa codes
• Update `code_is_valid` to check redirect targets before determining validity
• Add test case for rule redirects (TCH002 in this case)

## Test Plan

<!-- How was it tested? -->

I have added a test case for rule redirects to `crates/ruff_linter/resources/test/fixtures/ruff/RUF102.py`.

---

_Comment by @github-actions[bot] on 2025-09-04 18:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7d6e148fe6ac61d1d99f449915b30772a0cee854/providers/edge3/src/airflow/providers/edge3/worker_api/auth.py#L34'>providers/edge3/src/airflow/providers/edge3/worker_api/auth.py:34:79:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
- <a href='https://github.com/apache/airflow/blob/7d6e148fe6ac61d1d99f449915b30772a0cee854/providers/edge3/src/airflow/providers/edge3/worker_api/datamodels.py#L28'>providers/edge3/src/airflow/providers/edge3/worker_api/datamodels.py:28:73:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF102 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:86 on 2025-09-08 15:53_

Should both `code.as_str()` usages be `code_str` instead?

---

_@amyreese reviewed on 2025-09-08 15:53_

---

_@TaKO8Ki reviewed on 2025-09-08 15:57_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:86 on 2025-09-08 15:57_

Thank you. You're right. I have fixed it.

---

_Review requested from @amyreese by @TaKO8Ki on 2025-09-10 08:46_

---

_Label `fixes` added by @amyreese on 2025-09-10 20:50_

---

_Label `suppression` added by @amyreese on 2025-09-10 20:50_

---

_Merged by @amyreese on 2025-09-10 21:27_

---

_Closed by @amyreese on 2025-09-10 21:27_

---

_Comment by @amyreese on 2025-09-10 21:27_

Thank you!

---
