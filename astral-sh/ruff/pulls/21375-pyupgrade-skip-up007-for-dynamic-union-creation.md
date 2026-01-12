```yaml
number: 21375
title: "[`pyupgrade`] Skip UP007 for dynamic Union creation (`UP007`)"
type: pull_request
state: open
author: danparizher
labels: []
assignees: []
base: main
head: fix-21347
created_at: 2025-11-11T04:47:49Z
updated_at: 2025-11-11T22:32:09Z
url: https://github.com/astral-sh/ruff/pull/21375
synced_at: 2026-01-12T15:57:22Z
```

# [`pyupgrade`] Skip UP007 for dynamic Union creation (`UP007`)

---

_@danparizher_

## Summary

Fix UP007 to skip dynamic `Union` creation where the slice is a runtime variable (fixes #21347).

The rule was incorrectly emitting "Use `X | Y` for type annotations" for cases like `Union[types]` where `types` is a variable, which cannot be converted to PEP 604 syntax since the types are determined at runtime.

## Problem Analysis

UP007 checks for `Union[...]` annotations that can be rewritten using PEP 604's `|` operator syntax. However, the rule was triggering even when the slice argument was a variable (e.g., `Union[types]`), which represents dynamic Union creation at runtime. This is invalid because:

1. The types are not known statically
2. PEP 604 syntax (`X | Y`) requires compile-time type literals
3. The suggested fix cannot be applied

The rule should only apply to static type annotations with literal tuples or type literals that can be statically analyzed.

## Approach

Added an early return check in `non_pep604_annotation` for the `Union` operator branch to skip emitting diagnostics when the slice is an `Expr::Name` (variable). This prevents false positives for dynamic Union creation while preserving correct behavior for static type annotations.

The fix is minimal and targeted - it only adds a guard clause before the diagnostic is emitted, ensuring that dynamic Union creation cases are filtered out early.

## Test Plan

Added a regression test case in `UP007.py` that demonstrates dynamic Union creation (`Union[types]` where `types` is a variable) should not trigger UP007. All existing pyupgrade tests pass (151 tests), confirming the fix doesn't break existing functionality.

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 04:58_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4bde26ffb54928821176b8412a823b46ae2bcc16/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L1049'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:1049:74:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/637bb1cbbc93d3191ed5d91ae48c1038c637662f/libs/langchain_v1/langchain/agents/middleware/tool_selection.py#L64'>libs/langchain_v1/langchain/agents/middleware/tool_selection.py:64:78:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L368'>tests/types/test_type_hints.py:368:32:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L369'>tests/types/test_type_hints.py:369:34:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L373'>tests/types/test_type_hints.py:373:34:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/3c3ddc2a19f2385176c86d6feda14ecbe9abc790/reflex/utils/types.py#L240'>reflex/utils/types.py:240:32:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4bde26ffb54928821176b8412a823b46ae2bcc16/providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py#L1049'>providers/amazon/src/airflow/providers/amazon/aws/hooks/base_aws.py:1049:74:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/637bb1cbbc93d3191ed5d91ae48c1038c637662f/libs/langchain_v1/langchain/agents/middleware/tool_selection.py#L64'>libs/langchain_v1/langchain/agents/middleware/tool_selection.py:64:78:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L368'>tests/types/test_type_hints.py:368:32:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L369'>tests/types/test_type_hints.py:369:34:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/mlflow/mlflow/blob/b50e8aeab49cada9e1a92a7d885b252f572ea940/tests/types/test_type_hints.py#L373'>tests/types/test_type_hints.py:373:34:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/3c3ddc2a19f2385176c86d6feda14ecbe9abc790/reflex/utils/types.py#L240'>reflex/utils/types.py:240:32:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @dylwil3 on 2025-11-11 15:12_

Thanks for working on this! I'm not sure that this is fixing the root of the problem - for example it wouldn't catch `Union[foo()]` where `foo` returns a tuple of types. Instead, I think we should only be applying this lint in type annotation positions, as @ntBre suggested in the linked issue.

On an unrelated note: I personally do not find the AI-generated summaries in your PRs to be helpful. First, they are unnecessarily verbose and often include overly-confident or misleading statements. Second, they do not give me much confidence that a human being has reviewed the code changes. I would much prefer to read a concise summary that you wrote yourself.

---

_Comment by @danparizher on 2025-11-11 22:26_

Thanks for the feedback @dylwil3, I'll write the PR descriptions myself going forward and be more attentive to the code changes that AI generates.

Regarding this PR, I caught myself up in the linked issue and made a few changes following the discussion. Let me know if this revision is on the right track!

---
