```yaml
number: 7119
title: "Avoid triggering N806 on `TypeAlias` assignments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/N806
created_at: 2023-09-03T22:54:55Z
updated_at: 2023-09-04T08:51:36Z
url: https://github.com/astral-sh/ruff/pull/7119
synced_at: 2026-01-12T15:55:23Z
```

# Avoid triggering N806 on `TypeAlias` assignments

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ruff/issues/7068, although note that this solution relies on assignments being marked as `TypeAlias`, which is the best we can do for now.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-09-03 22:55_

---

_Merged by @charliermarsh on 2023-09-04 08:44_

---

_Closed by @charliermarsh on 2023-09-04 08:44_

---

_Branch deleted on 2023-09-04 08:44_

---

_Comment by @github-actions[bot] on 2023-09-04 08:51_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -2, 0 error(s))

<details><summary>zulip (+0, -2)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/81bd63cb46273b8c94ef9e92c00893ed97110119/analytics/management/commands/populate_analytics_db.py#L151'>analytics/management/commands/populate_analytics_db.py:151:9:</a> N806 Variable `FixtureData` in function should be lowercase
- <a href='https://github.com/zulip/zulip/blob/81bd63cb46273b8c94ef9e92c00893ed97110119/analytics/views/stats.py#L246'>analytics/views/stats.py:246:5:</a> N806 Variable `TableType` in function should be lowercase
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| N806 | 2 | 0 | 2 |



---
