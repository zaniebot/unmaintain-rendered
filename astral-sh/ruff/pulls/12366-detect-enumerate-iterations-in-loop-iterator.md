```yaml
number: 12366
title: "Detect enumerate iterations in `loop-iterator-mutation`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/B9
created_at: 2024-07-17T14:21:31Z
updated_at: 2024-07-17T16:03:38Z
url: https://github.com/astral-sh/ruff/pull/12366
synced_at: 2026-01-10T21:47:02Z
```

# Detect enumerate iterations in `loop-iterator-mutation`

---

_Pull request opened by @charliermarsh on 2024-07-17 14:21_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12164.


---

_Label `rule` added by @charliermarsh on 2024-07-17 14:21_

---

_Comment by @github-actions[bot] on 2024-07-17 14:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/79722e3deeffdd4d61c6e5f377baa6c4d2296fc5/airflow/utils/cli.py#L150'>airflow/utils/cli.py:150:17:</a> B909 Mutation to loop iterable `full_command` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/6a0657e5959ba1777c4d427f8f355d499035d145/mypy/typeops.py#L949'>mypy/typeops.py:949:21:</a> B909 Mutation to loop iterable `proper_types` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/zerver/lib/markdown/nested_code_blocks.py#L82'>zerver/lib/markdown/nested_code_blocks.py:82:17:</a> B909 Mutation to loop iterable `parent` during iteration
+ <a href='https://github.com/zulip/zulip/blob/08d48d3d972e57564fbf36ea8ec1273be29b85f3/zerver/lib/markdown/nested_code_blocks.py#L83'>zerver/lib/markdown/nested_code_blocks.py:83:17:</a> B909 Mutation to loop iterable `parent` during iteration
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/d489247505a953885a156e61d4473497cbc167ea/scripts/release.py#L54'>scripts/release.py:54:17:</a> B909 Mutation to loop iterable `lines` during iteration
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B909 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-07-17 14:37_

I need to allow assignments to the current key.

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-17 15:24_

---

_Merged by @charliermarsh on 2024-07-17 16:03_

---

_Closed by @charliermarsh on 2024-07-17 16:03_

---

_Branch deleted on 2024-07-17 16:03_

---
