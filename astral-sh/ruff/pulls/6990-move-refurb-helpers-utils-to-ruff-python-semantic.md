```yaml
number: 6990
title: "Move `refurb/helpers` utils to `ruff_python_semantic` for broader use"
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: ref/moverefurbutils
created_at: 2023-08-29T18:27:57Z
updated_at: 2023-08-29T19:20:15Z
url: https://github.com/astral-sh/ruff/pull/6990
synced_at: 2026-01-12T15:55:23Z
```

# Move `refurb/helpers` utils to `ruff_python_semantic` for broader use

---

_@qdegraaf_

## Summary

The utils added for `refurb` in its `helpers.rs` file could be useful for many other plugins. (Such as the PERF4XX codes, see e.g. https://github.com/astral-sh/ruff/pull/6132 ).

This PR moves them to `ruff_python_semantic::analyzers::typing` as suggested in https://github.com/astral-sh/ruff/pull/6132#issuecomment-1697910093

## Test Plan

Confirmed `refurb` and all other tests still work 


---

_Comment by @github-actions[bot] on 2023-08-29 18:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-08-29 18:44_

Thanks!

---

_Merged by @charliermarsh on 2023-08-29 18:45_

---

_Closed by @charliermarsh on 2023-08-29 18:45_

---

_Branch deleted on 2023-08-29 19:20_

---
