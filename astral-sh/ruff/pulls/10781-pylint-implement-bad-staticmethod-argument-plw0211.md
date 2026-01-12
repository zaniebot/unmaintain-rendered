```yaml
number: 10781
title: "[`pylint`] Implement `bad-staticmethod-argument` (`PLW0211`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: bad-staticmethod-argument
created_at: 2024-04-04T22:57:29Z
updated_at: 2024-04-05T22:10:45Z
url: https://github.com/astral-sh/ruff/pull/10781
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] Implement `bad-staticmethod-argument` (`PLW0211`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `bad-staticmethod-argument` from pylint, part of #970.

## Test Plan

Text fixture added.


---

_Comment by @github-actions[bot] on 2024-04-04 23:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1f03b9c86c7dd5937c7d32976d8653979e2f7e41/tests/secrets/test_cache.py#L43'>tests/secrets/test_cache.py:43:25:</a> PLW0211 First argument of a static method should not be named `self`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/web/http_api/hooks/base.py#L78'>indico/web/http_api/hooks/base.py:78:18:</a> PLW0211 First argument of a static method should not be named `cls`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0211 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-05 21:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-05 21:14_

---

_Label `rule` added by @charliermarsh on 2024-04-05 21:14_

---

_Label `preview` added by @charliermarsh on 2024-04-05 21:14_

---

_Merged by @charliermarsh on 2024-04-05 21:33_

---

_Closed by @charliermarsh on 2024-04-05 21:33_

---

_Comment by @charliermarsh on 2024-04-05 21:38_

Thanks!

---

_Branch deleted on 2024-04-05 22:10_

---
