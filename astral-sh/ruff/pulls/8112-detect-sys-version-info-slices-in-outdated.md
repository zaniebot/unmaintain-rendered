```yaml
number: 8112
title: "Detect `sys.version_info` slices in `outdated-version-block`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2023-10-21T23:00:28Z
updated_at: 2023-10-21T23:15:39Z
url: https://github.com/astral-sh/ruff/pull/8112
synced_at: 2026-01-12T02:32:41Z
```

# Detect `sys.version_info` slices in `outdated-version-block`

---

_Pull request opened by @charliermarsh on 2023-10-21 23:00_

## Summary

Given `sys.version_info[:2] >= (3,0)`, we should treat this equivalently to `sys.version_info >= (3,0)`.

Closes https://github.com/astral-sh/ruff/issues/8095.


---

_Label `bug` added by @charliermarsh on 2023-10-21 23:00_

---

_Merged by @charliermarsh on 2023-10-21 23:08_

---

_Closed by @charliermarsh on 2023-10-21 23:08_

---

_Branch deleted on 2023-10-21 23:08_

---

_Comment by @github-actions[bot] on 2023-10-21 23:15_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>mypy (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/mypy/blob/27c4b462aa4cf269397253eca7a88e7fbbf4e43e/mypy/util.py#L413'>mypy/util.py:413:8:</a> UP036 Version block is outdated for minimum Python version
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| UP036 | 1 | 1 | 0 |



---
