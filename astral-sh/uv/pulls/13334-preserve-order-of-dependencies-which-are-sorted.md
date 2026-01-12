```yaml
number: 13334
title: Preserve order of dependencies which are sorted naively
type: pull_request
state: merged
author: RazerM
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feature/preserve-naive-dependency-sort
created_at: 2025-05-07T17:58:35Z
updated_at: 2025-05-08T07:35:12Z
url: https://github.com/astral-sh/uv/pull/13334
synced_at: 2026-01-12T16:10:39Z
```

# Preserve order of dependencies which are sorted naively

---

_@RazerM_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The prior implementation only looks for dependencies which are sorted by name then specifier.

I knew uv was meant to preserve sorted dependencies, but it never seemed to work for me.

I've always used the "sort lines" feature of PyCharm/Sublime to sort these lists, and I guess I'm not the only one. In such a case, `flask-wtf>=1.2.1` is sorted before `flask>=3.0.2`.

After digging into the code I realised what was happening, hence this merge request.

Maybe there's a tool I'm not aware of that people are using to sort dependencies "properly", or are doing it by hand, but I think this is worth supporting.

Relevant issues: https://github.com/astral-sh/uv/issues/9076, https://github.com/astral-sh/uv/issues/10738

## Test Plan

`cargo test`

---

_@charliermarsh approved on 2025-05-07 20:18_

That's a good idea!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-07 21:55_

---

_Merged by @charliermarsh on 2025-05-07 22:18_

---

_Closed by @charliermarsh on 2025-05-07 22:18_

---

_Label `enhancement` added by @charliermarsh on 2025-05-07 22:18_

---

_Branch deleted on 2025-05-08 07:35_

---
