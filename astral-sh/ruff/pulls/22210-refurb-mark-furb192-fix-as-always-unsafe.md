```yaml
number: 22210
title: "[`refurb`] Mark `FURB192` fix as always unsafe"
type: pull_request
state: merged
author: harupy
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb192-always-unsafe
created_at: 2025-12-26T15:49:03Z
updated_at: 2025-12-29T14:17:34Z
url: https://github.com/astral-sh/ruff/pull/22210
synced_at: 2026-01-12T15:57:44Z
```

# [`refurb`] Mark `FURB192` fix as always unsafe

---

_@harupy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Close #22204

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

Existing checks

---

_Renamed from "[`refurb`] Mark FURB192 fix as always unsafe" to "[`refurb`] Mark `FURB192` fix as always unsafe" by @harupy on 2025-12-26 15:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 15:57_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -10 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/providers/yandex/src/airflow/providers/yandex/secrets/lockbox.py#L259'>providers/yandex/src/airflow/providers/yandex/secrets/lockbox.py:259:16:</a> FURB192 Prefer `min` over `sorted()` to compute the minimum value in a sequence
- <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/providers/yandex/src/airflow/providers/yandex/secrets/lockbox.py#L259'>providers/yandex/src/airflow/providers/yandex/secrets/lockbox.py:259:16:</a> FURB192 [*] Prefer `min` over `sorted()` to compute the minimum value in a sequence
+ <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/scripts/ci/prek/upgrade_important_versions.py#L111'>scripts/ci/prek/upgrade_important_versions.py:111:30:</a> FURB192 Prefer `max` over `sorted()` to compute the maximum value in a sequence
- <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/scripts/ci/prek/upgrade_important_versions.py#L111'>scripts/ci/prek/upgrade_important_versions.py:111:30:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
+ <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/scripts/ci/prek/upgrade_important_versions.py#L150'>scripts/ci/prek/upgrade_important_versions.py:150:22:</a> FURB192 Prefer `max` over `sorted()` to compute the maximum value in a sequence
- <a href='https://github.com/apache/airflow/blob/1cda358d13cb2d0d3f1662d782d7a18a04554e5e/scripts/ci/prek/upgrade_important_versions.py#L150'>scripts/ci/prek/upgrade_important_versions.py:150:22:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/1834c5bb757c215cf9eb8f98f994a5843618ae6b/src/scikit_build_core/file_api/reply.py#L37'>src/scikit_build_core/file_api/reply.py:37:22:</a> FURB192 Prefer `max` over `sorted()` to compute the maximum value in a sequence
- <a href='https://github.com/scikit-build/scikit-build-core/blob/1834c5bb757c215cf9eb8f98f994a5843618ae6b/src/scikit_build_core/file_api/reply.py#L37'>src/scikit_build_core/file_api/reply.py:37:22:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/55f89b0ac34efc86a06c378297c35ec4ae3349d6/astropy/time/core.py#L2372'>astropy/time/core.py:2372:21:</a> FURB192 Prefer `max` over `sorted()` to compute the maximum value in a sequence
- <a href='https://github.com/astropy/astropy/blob/55f89b0ac34efc86a06c378297c35ec4ae3349d6/astropy/time/core.py#L2372'>astropy/time/core.py:2372:21:</a> FURB192 [*] Prefer `max` over `sorted()` to compute the maximum value in a sequence
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB192 | 10 | 0 | 0 | 0 | 10 |

</p>
</details>





---

_Label `fixes` added by @ntBre on 2025-12-29 14:15_

---

_Label `preview` added by @ntBre on 2025-12-29 14:15_

---

_@ntBre approved on 2025-12-29 14:16_

Perfect, thank you!

---

_Merged by @ntBre on 2025-12-29 14:17_

---

_Closed by @ntBre on 2025-12-29 14:17_

---
