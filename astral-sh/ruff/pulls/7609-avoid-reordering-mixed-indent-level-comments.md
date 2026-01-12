```yaml
number: 7609
title: Avoid reordering mixed-indent-level comments after branches
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/comment-placement
created_at: 2023-09-22T19:09:07Z
updated_at: 2023-09-22T22:12:32Z
url: https://github.com/astral-sh/ruff/pull/7609
synced_at: 2026-01-12T15:55:24Z
```

# Avoid reordering mixed-indent-level comments after branches

---

_@charliermarsh_

## Summary

Given:

```python
if True:
    if True:
        if True:
            pass

        #a
            #b
        #c
else:
    pass
```

When determining the placement of the various comments, we compute the indentation depth of each comment, and then compare it to the depth of the previous statement. It turns out this can lead to reordering comments, e.g., above, `#b` is assigned as a trailing comment of `pass`, and so gets reordered above `#a`.

This PR modifies the logic such that when we compute the indentation depth of `#b`, we limit it to at most the indentation depth of `#a`. In other words, when analyzing comments at the end of branches, we don't let successive comments go any _deeper_ than their preceding comments.

Closes https://github.com/astral-sh/ruff/issues/7602.

## Test Plan

`cargo test`

No change in similarity.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |

After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99963 |              2587 |               319 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-22 19:09_

---

_Review requested from @konstin by @charliermarsh on 2023-09-22 19:09_

---

_Label `bug` added by @charliermarsh on 2023-09-22 19:09_

---

_Label `formatter` added by @charliermarsh on 2023-09-22 19:09_

---

_@charliermarsh reviewed on 2023-09-22 19:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:821 on 2023-09-22 19:09_

Hmm, is there an easier way to access the comments that we care about here, rather than re-lexing?

---

_@charliermarsh reviewed on 2023-09-22 19:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__if.py.snap`:558 on 2023-09-22 19:09_

This is a good change, IMO -- the previous result was reordering comments.

---

_@konstin reviewed on 2023-09-22 19:14_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:821 on 2023-09-22 19:14_

We could pass in the in-building comment map, but that would break isolation quite a bit

---

_@konstin reviewed on 2023-09-22 19:17_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:821 on 2023-09-22 19:17_

we wouldn't even the lexer here, just the first non-whitespace position, i'm not sure though if manual is better here than the simple tokenizer

---

_@konstin approved on 2023-09-22 19:18_

i can't come up with a better solution

---

_Merged by @charliermarsh on 2023-09-22 22:12_

---

_Closed by @charliermarsh on 2023-09-22 22:12_

---

_Branch deleted on 2023-09-22 22:12_

---
