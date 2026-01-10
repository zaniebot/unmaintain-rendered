```yaml
number: 14209
title: "[`refurb`] Use `UserString` instead of non-existent `UserStr`"
type: pull_request
state: merged
author: nijel
labels:
  - bug
assignees: []
merged: true
base: main
head: userstring
created_at: 2024-11-08T18:49:39Z
updated_at: 2024-11-09T05:48:46Z
url: https://github.com/astral-sh/ruff/pull/14209
synced_at: 2026-01-10T20:50:57Z
```

# [`refurb`] Use `UserString` instead of non-existent `UserStr`

---

_Pull request opened by @nijel on 2024-11-08 18:49_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The class name is UserString, not a UserStr, see https://docs.python.org/3.9/library/collections.html#collections.UserString

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-11-08 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9a5a80e6df50e17d28b2d2ceacece7f9e151b78f/kubernetes_tests/test_base.py#L45'>kubernetes_tests/test_base.py:45:26:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/apache/airflow/blob/9a5a80e6df50e17d28b2d2ceacece7f9e151b78f/kubernetes_tests/test_base.py#L45'>kubernetes_tests/test_base.py:45:26:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserString` instead
- <a href='https://github.com/apache/airflow/blob/9a5a80e6df50e17d28b2d2ceacece7f9e151b78f/tests/utils/log/test_secrets_masker.py#L317'>tests/utils/log/test_secrets_masker.py:317:54:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/apache/airflow/blob/9a5a80e6df50e17d28b2d2ceacece7f9e151b78f/tests/utils/log/test_secrets_masker.py#L317'>tests/utils/log/test_secrets_masker.py:317:54:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserString` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/4c2392e55c7079b8eb0e11d3227cfd24890872e3/libs/core/tests/unit_tests/stubs.py#L7'>libs/core/tests/unit_tests/stubs.py:7:14:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/langchain-ai/langchain/blob/4c2392e55c7079b8eb0e11d3227cfd24890872e3/libs/core/tests/unit_tests/stubs.py#L7'>libs/core/tests/unit_tests/stubs.py:7:14:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserString` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/bfbd625ac0fb2d1b68b3c7754ed0d1fb1cef0e5e/src/scikit_build_core/settings/skbuild_model.py#L31'>src/scikit_build_core/settings/skbuild_model.py:31:27:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/bfbd625ac0fb2d1b68b3c7754ed0d1fb1cef0e5e/src/scikit_build_core/settings/skbuild_model.py#L31'>src/scikit_build_core/settings/skbuild_model.py:31:27:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserString` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/462c88866ede7aa2484f2173e8cd6b5c364a0003/indico/legacy/pdfinterface/latex.py#L169'>indico/legacy/pdfinterface/latex.py:169:16:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/indico/indico/blob/462c88866ede7aa2484f2173e8cd6b5c364a0003/indico/legacy/pdfinterface/latex.py#L169'>indico/legacy/pdfinterface/latex.py:169:16:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserString` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB189 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>




---

_Comment by @sbrugman on 2024-11-08 19:32_

Thanks!

---

_@charliermarsh approved on 2024-11-09 01:53_

Thanks, whoops!

---

_Label `bug` added by @charliermarsh on 2024-11-09 01:53_

---

_Renamed from "Fix FURB189 replacing str to UserString" to "[`refurb`] Use `UserString` instead of non-existent `UserStr`" by @charliermarsh on 2024-11-09 01:54_

---

_Merged by @charliermarsh on 2024-11-09 01:54_

---

_Closed by @charliermarsh on 2024-11-09 01:54_

---

_Branch deleted on 2024-11-09 05:48_

---
