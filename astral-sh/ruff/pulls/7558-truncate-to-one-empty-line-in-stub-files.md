```yaml
number: 7558
title: Truncate to one empty line in stub files
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/pyi
created_at: 2023-09-21T00:00:39Z
updated_at: 2023-09-21T20:24:43Z
url: https://github.com/astral-sh/ruff/pull/7558
synced_at: 2026-01-12T02:39:10Z
```

# Truncate to one empty line in stub files

---

_Pull request opened by @charliermarsh on 2023-09-21 00:00_

## Summary

This PR modifies a variety of sites in which we insert up to two empty lines to instead truncate to at most one empty line in stub files. We already enforce this in _some_ places, but not all.

## Test Plan

`cargo test`

No changes in similarity (as expected, since this only impacts unformatted `.pyi` files).

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               323 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Label `formatter` added by @charliermarsh on 2023-09-21 00:04_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-21 00:10_

---

_Review requested from @konstin by @charliermarsh on 2023-09-21 00:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:318 on 2023-09-21 06:08_

Should we introduce a helper for this, considering that it is spread across multiple files in many different places?

---

_@MichaReiser approved on 2023-09-21 06:08_

---

_Comment by @MichaReiser on 2023-09-21 06:08_

I'll let @konstin do the final review as the *empty lines in stub files* expert :)

---

_@konstin approved on 2023-09-21 19:37_

good catch

---

_Merged by @charliermarsh on 2023-09-21 20:24_

---

_Closed by @charliermarsh on 2023-09-21 20:24_

---

_Branch deleted on 2023-09-21 20:24_

---
