```yaml
number: 7047
title: Fix placement for comments within f-strings concatenations
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format
created_at: 2023-09-01T16:12:21Z
updated_at: 2023-09-01T16:27:33Z
url: https://github.com/astral-sh/ruff/pull/7047
synced_at: 2026-01-12T15:55:23Z
```

# Fix placement for comments within f-strings concatenations

---

_@charliermarsh_

## Summary

Restores the dangling comment handling for f-strings, which broke with the parenthesized expression code.

Closes https://github.com/astral-sh/ruff/issues/6898.

## Test Plan

`cargo test`

No change in any of the similarity indexes or changed file counts:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99957 |              2760 |                67 |
| transformers |           0.99927 |              2587 |               468 |
| twine        |           0.99982 |                33 |                 1 |
| typeshed     |           0.99978 |              3496 |              2173 |
| warehouse    |           0.99818 |               648 |                24 |
| zulip        |           0.99942 |              1437 |                32 |

---

_Marked ready for review by @charliermarsh on 2023-09-01 16:12_

---

_Comment by @charliermarsh on 2023-09-01 16:13_

\cc @davidszotten 

---

_Label `formatter` added by @charliermarsh on 2023-09-01 16:14_

---

_Comment by @davidszotten on 2023-09-01 16:19_

note that black formats your first 3 examples differently (this may be due to existing/known differences)

```
(f"{1}" f"{2}")  # comment
(f"{1}" f"{2}")  # comment
(1, (f"{2}"))  # comment
```

---

_Comment by @charliermarsh on 2023-09-01 16:20_

@davidszotten - Ah thank you for checking that! It does make sense to me since we tend to break on trailing comments unlike Black, so I don't think it needs to block here.

---

_Merged by @charliermarsh on 2023-09-01 16:27_

---

_Closed by @charliermarsh on 2023-09-01 16:27_

---

_Branch deleted on 2023-09-01 16:27_

---
