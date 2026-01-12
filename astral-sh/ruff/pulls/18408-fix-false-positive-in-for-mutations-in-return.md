```yaml
number: 18408
title: Fix false positive in  for mutations in return statements (B909)
type: pull_request
state: merged
author: K-dash
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/b909-false-positive-return-mutation
created_at: 2025-06-01T04:42:21Z
updated_at: 2025-06-13T14:39:56Z
url: https://github.com/astral-sh/ruff/pull/18408
synced_at: 2026-01-12T15:56:18Z
```

# Fix false positive in  for mutations in return statements (B909)

---

_@K-dash_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes false positive in B909 (`loop-iterator-mutation`) where mutations inside return/break statements were incorrectly flagged as violations.
The fix adds tracking for when mutations occur within return/break statements and excludes them from violation detection, as they don't cause the iteration issues B909 is designed to prevent.



## Test Plan

  - Added test cases covering the reported false positive scenarios to `B909.py`
  - Verified existing B909 tests continue to pass (no regressions)
  - Ran `cargo test -p ruff_linter --lib flake8_bugbear` successfully

Fixes #18399

---

_Label `rule` added by @ntBre on 2025-06-01 14:22_

---

_Review requested from @ntBre by @ntBre on 2025-06-01 14:22_

---

_Comment by @github-actions[bot] on 2025-06-01 14:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +26 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:22:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py#L78'>airflow-core/src/airflow/api_fastapi/auth/managers/base_auth_manager.py:78:22:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L37'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:37:23:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/common/exceptions.py#L37'>airflow-core/src/airflow/api_fastapi/common/exceptions.py:37:23:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/core_api/base.py#L52'>airflow-core/src/airflow/api_fastapi/core_api/base.py:52:16:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/apache/airflow/blob/aae99f2aa6b3764fe2fcf6cb0e44051d3eb15ea5/airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py#L37'>airflow-core/src/airflow/api_fastapi/core_api/services/public/common.py:37:18:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -0 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/output_parsers/base.py#L31'>libs/core/langchain_core/output_parsers/base.py:31:26:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/output_parsers/base.py#L31'>libs/core/langchain_core/output_parsers/base.py:31:26:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/prompts/base.py#L45'>libs/core/langchain_core/prompts/base.py:45:25:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/prompts/base.py#L45'>libs/core/langchain_core/prompts/base.py:45:25:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/runnables/base.py#L111'>libs/core/langchain_core/runnables/base.py:111:15:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/runnables/base.py#L111'>libs/core/langchain_core/runnables/base.py:111:15:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/stores.py#L26'>libs/core/langchain_core/stores.py:26:16:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/langchain-ai/langchain/blob/5839801897c798b7a516f9a6c415bead2386209f/libs/core/langchain_core/stores.py#L26'>libs/core/langchain_core/stores.py:26:16:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/30315fdd1b2c95444911010b60e9086b8cceba50/src/latch/types/metadata.py#L440'>src/latch/types/metadata.py:440:25:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/latchbio/latch/blob/30315fdd1b2c95444911010b60e9086b8cceba50/src/latch/types/metadata.py#L440'>src/latch/types/metadata.py:440:25:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/latchbio/latch/blob/30315fdd1b2c95444911010b60e9086b8cceba50/src/latch/types/metadata.py#L489'>src/latch/types/metadata.py:489:24:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/latchbio/latch/blob/30315fdd1b2c95444911010b60e9086b8cceba50/src/latch/types/metadata.py#L489'>src/latch/types/metadata.py:489:24:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/ecd5141cc036366cc9e3ca371096d6a14b0ccd13/stdlib/asyncio/queues.pyi#L28'>stdlib/asyncio/queues.pyi:28:12:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/python/typeshed/blob/ecd5141cc036366cc9e3ca371096d6a14b0ccd13/stdlib/asyncio/queues.pyi#L28'>stdlib/asyncio/queues.pyi:28:12:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a25000298fe5099ed6e47104449aba100e086ae9/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/zulip/zulip/blob/a25000298fe5099ed6e47104449aba100e086ae9/zerver/lib/notes.py#L12'>zerver/lib/notes.py:12:16:</a> PYI059 `Generic[]` should always be the last base class
+ <a href='https://github.com/zulip/zulip/blob/a25000298fe5099ed6e47104449aba100e086ae9/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 [*] `Generic[]` should always be the last base class
- <a href='https://github.com/zulip/zulip/blob/a25000298fe5099ed6e47104449aba100e086ae9/zerver/lib/queue.py#L35'>zerver/lib/queue.py:35:18:</a> PYI059 `Generic[]` should always be the last base class
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/9e9633de9da7a9fab03b4bba3a326bf85b412050/src/_pytest/recwarn.py#L214'>src/_pytest/recwarn.py:214:24:</a> B909 Mutation to loop iterable `self._list` during iteration
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI059 | 26 | 0 | 0 | 26 | 0 |
| B909 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:294 on 2025-06-03 21:22_

If we're not going to add the mutations anyway, can we simply skip the `walk_stmt` call here?
```suggestion
```

I tried this locally and it seems to work, but maybe there's an edge case I'm missing. I even found the [commit](https://github.com/astral-sh/ruff/pull/9578/commits/8fe10406c5a80c76af2233de533be1847cac9e53) from the initial PR that added the rule, but I didn't see any reasoning for including it, so hopefully we're safe to skip it if all the tests pass.

---

_@ntBre reviewed on 2025-06-03 21:35_

Thanks! The new tests look great, but I think there might be a simpler fix for the issue.

---

_@K-dash reviewed on 2025-06-03 22:27_

---

_Review comment by @K-dash on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:294 on 2025-06-03 22:27_

You're absolutely right. 
Since we're not walking the statement at all, there's no need for the flag-based approach. The mutations simply won't be detected in the first place because we skip traversing the expressions within return/break statements entirely.

I've simplified the implementation to just skip the `walk_stmt` call. Thanks for the review!

---

_Review requested from @ntBre by @K-dash on 2025-06-03 22:27_

---

_@ntBre reviewed on 2025-06-04 02:25_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:148 on 2025-06-04 02:25_

I think we should also revert this field addition. I'm a bit surprised clippy didn't catch it, but I suppose it does look used.

---

_Review comment by @K-dash on `crates/ruff_linter/src/rules/flake8_bugbear/rules/loop_iterator_mutation.rs`:148 on 2025-06-04 04:42_

Oh... You're totally right. I've already reverted the field addition and simplified the implementation. The `in_return_or_break` field has been removed along with all its associated logic. Thanks for catching that!

---

_@K-dash reviewed on 2025-06-04 04:42_

---

_Review requested from @ntBre by @K-dash on 2025-06-08 01:24_

---

_Comment by @K-dash on 2025-06-13 01:07_

@ntBre 
I was wondering if you've had a chance to review this yet?
I would greatly appreciate your feedback when you have the opportunity.

---

_@ntBre approved on 2025-06-13 14:25_

Excellent, thank you! It's always fun to fix a bug by deleting code :)

---

_Merged by @ntBre on 2025-06-13 14:39_

---

_Closed by @ntBre on 2025-06-13 14:39_

---
