```yaml
number: 8375
title: No newline after function docstrings
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: no-newline-after-function-docstrings
created_at: 2023-10-31T11:06:51Z
updated_at: 2023-10-31T18:32:16Z
url: https://github.com/astral-sh/ruff/pull/8375
synced_at: 2026-01-10T23:40:55Z
```

# No newline after function docstrings

---

_Pull request opened by @konstin on 2023-10-31 11:06_

Fixup for #8216 to not apply to function docstrings.

Main before #8216:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                33 |
| home-assistant |           0.99963 |             10596 |               148 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               328 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

main now:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                48 |
| home-assistant |           0.99963 |             10596 |               181 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               339 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                23 |

PR:

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                33 |
| home-assistant |           0.99963 |             10596 |               148 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               328 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

---

_Label `formatter` added by @konstin on 2023-10-31 11:06_

---

_Comment by @github-actions[bot] on 2023-10-31 11:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_@charliermarsh approved on 2023-10-31 18:31_

Thank you.

---

_Comment by @charliermarsh on 2023-10-31 18:31_

\cc @zanieb - Also expected ecosystem format changes here, just in case you need more examples.

---

_Merged by @charliermarsh on 2023-10-31 18:32_

---

_Closed by @charliermarsh on 2023-10-31 18:32_

---

_Branch deleted on 2023-10-31 18:32_

---
