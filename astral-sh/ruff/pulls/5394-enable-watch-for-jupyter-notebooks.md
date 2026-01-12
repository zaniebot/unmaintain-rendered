```yaml
number: 5394
title: Enable --watch for Jupyter notebooks
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/jupyter
created_at: 2023-06-27T16:15:24Z
updated_at: 2023-06-27T16:53:48Z
url: https://github.com/astral-sh/ruff/pull/5394
synced_at: 2026-01-12T15:55:18Z
```

# Enable --watch for Jupyter notebooks

---

_@charliermarsh_

## Summary

The list of extensions that support watching is hard-coded (unfortunately); this PR adds `.ipynb` to the list.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-27 16:15_

---

_Comment by @github-actions[bot] on 2023-06-27 16:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-06-27 16:46_

is there a specific reason we don't use the normal include glob here ?

---

_Comment by @charliermarsh on 2023-06-27 16:53_

I think it's because it's not trivial to determine the "right" set of includes for a given path at this point in the code. (We could use different includes for different subdirectories.)

---

_Merged by @charliermarsh on 2023-06-27 16:53_

---

_Closed by @charliermarsh on 2023-06-27 16:53_

---

_Branch deleted on 2023-06-27 16:53_

---
