```yaml
number: 15463
title: "[airflow] replace typo \"security_managr\" as \"security_manager\" (AIR303)"
type: pull_request
state: merged
author: Lee-W
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: fix-airflow-typo
created_at: 2025-01-13T23:25:23Z
updated_at: 2025-01-23T08:50:04Z
url: https://github.com/astral-sh/ruff/pull/15463
synced_at: 2026-01-10T20:05:43Z
```

# [airflow] replace typo "security_managr" as "security_manager" (AIR303)

---

_Pull request opened by @Lee-W on 2025-01-13 23:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Replace typo "security_managr" in AIR303 as "security_manager"

## Test Plan

<!-- How was it tested? -->

a test fixture has been updated


---

_Renamed from "fix(airflow): AIR303" to "[airflow] replace typo "security_managr" as "security_manager" (AIR303)" by @Lee-W on 2025-01-13 23:27_

---

_Comment by @github-actions[bot] on 2025-01-13 23:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

```
Failed to clone apache/superset: error: RPC failed; HTTP 500 curl 22 The requested URL returned error: 500
fatal: expected 'packfile'
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

```
Failed to pull: error: RPC failed; HTTP 500 curl 22 The requested URL returned error: 500
fatal: expected flush after ref listing
```

</p>
</details>




---

_@charliermarsh approved on 2025-01-13 23:37_

---

_Label `bug` added by @charliermarsh on 2025-01-13 23:37_

---

_Label `preview` added by @charliermarsh on 2025-01-13 23:37_

---

_Merged by @charliermarsh on 2025-01-13 23:38_

---

_Closed by @charliermarsh on 2025-01-13 23:38_

---

_Branch deleted on 2025-01-23 08:50_

---
