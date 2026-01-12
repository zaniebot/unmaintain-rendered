```yaml
number: 7557
title: Allow up to two newlines after trailing clause body comments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/newlines
created_at: 2023-09-20T23:47:13Z
updated_at: 2023-09-21T14:04:51Z
url: https://github.com/astral-sh/ruff/pull/7557
synced_at: 2026-01-12T02:39:10Z
```

# Allow up to two newlines after trailing clause body comments

---

_Pull request opened by @charliermarsh on 2023-09-20 23:47_

## Summary

The number of newlines after a trailing comment in a clause body needs to follow the usual rules -- so, up to two for top-level, up to one for nested, etc.

For example, Black preserves both newlines after `# comment` here:

```python
if True:
    pass

    # comment


else:
    pass
```

But it truncates to one newline here:

```python
if True:
    if True:
        pass
        # comment


    else:
        pass
else:
    pass
```

## Test Plan

Significant improvement on `transformers`.

Before:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| transformers |           0.99957 |              2587 |               402 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


After:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1631 |
| django       |           0.99983 |              2760 |                36 |
| **transformers** |           **0.99963** |              **2587** |               **323** |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99979 |              3496 |                22 |
| warehouse    |           0.99967 |               648 |                15 |
| zulip        |           0.99972 |              1437 |                21 |


---

_Label `formatter` added by @charliermarsh on 2023-09-20 23:47_

---

_Comment by @codspeed-hq[bot] on 2023-09-20 23:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/newlines)

### Merging #7557 will **not alter performance**

<sub>Comparing <code>charlie/newlines</code> (cf36f01) with <code>main</code> (124d95d)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-21 00:01_

---

_Review requested from @konstin by @charliermarsh on 2023-09-21 00:01_

---

_@MichaReiser approved on 2023-09-21 06:03_

That wasn't as tricky as we feard

---

_@konstin approved on 2023-09-21 08:01_

---

_Merged by @charliermarsh on 2023-09-21 14:04_

---

_Closed by @charliermarsh on 2023-09-21 14:04_

---

_Branch deleted on 2023-09-21 14:04_

---
