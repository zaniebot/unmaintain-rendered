```yaml
number: 7981
title: Update and extend formatter ecosystem checks
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: new-ecosystem-checks
created_at: 2023-10-16T08:39:26Z
updated_at: 2023-10-16T11:24:43Z
url: https://github.com/astral-sh/ruff/pull/7981
synced_at: 2026-01-12T02:32:41Z
```

# Update and extend formatter ecosystem checks

---

_Pull request opened by @konstin on 2023-10-16 08:39_

**Summary** Adds home-assistant, a project with 10k files, and poetry, which uses preview style, to the ecosystem checks.

Update all revisions to latest main.

Old:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76047 |              1789 |              1632 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


New:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.76382 |              1799 |              1436 |
| django         |           0.99983 |              2772 |                31 |
| home-assistant |           0.99950 |             10596 |               165 |
| poetry         |           0.99944 |               317 |                 8 |
| transformers   |           0.99961 |              2657 |               295 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99974 |              3669 |                19 |
| warehouse      |           0.99971 |               654 |                13 |
| zulip          |           0.99972 |              1459 |                13 |


---

_Label `internal` added by @konstin on 2023-10-16 08:39_

---

_Label `formatter` added by @konstin on 2023-10-16 08:39_

---

_Comment by @github-actions[bot] on 2023-10-16 09:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-10-16 11:20_

Does any of the existing projects have more deviations after upgrading?

---

_Comment by @konstin on 2023-10-16 11:24_

Added base results, looks like only the expected fluctuation

---

_Merged by @konstin on 2023-10-16 11:24_

---

_Closed by @konstin on 2023-10-16 11:24_

---

_Branch deleted on 2023-10-16 11:24_

---
