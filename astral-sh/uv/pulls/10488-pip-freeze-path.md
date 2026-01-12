```yaml
number: 10488
title: Pip freeze path
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - enhancement
  - compatibility
  - cli
assignees: []
merged: true
base: main
head: pip-freeze-path
created_at: 2025-01-11T01:45:17Z
updated_at: 2025-01-13T22:50:05Z
url: https://github.com/astral-sh/uv/pull/10488
synced_at: 2026-01-12T16:09:19Z
```

# Pip freeze path

---

_@ericmarkmartin_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #5952

Add a `--path` option to `uv pip freeze` to be compatible with `pip freeze`

## Test Plan

New snapshot tests


---

_Marked ready for review by @ericmarkmartin on 2025-01-11 06:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-11 13:38_

---

_Review requested from @charliermarsh by @charliermarsh on 2025-01-11 13:38_

---

_Label `enhancement` added by @charliermarsh on 2025-01-11 13:38_

---

_Label `compatibility` added by @charliermarsh on 2025-01-11 13:38_

---

_Label `cli` added by @charliermarsh on 2025-01-11 13:38_

---

_Comment by @ericmarkmartin on 2025-01-11 23:15_

One thing to point out here, the behavior of `pip freeze --path` when passed a non-existent directory. I replicated this behavior [in the PR](https://github.com/astral-sh/uv/pull/10488/files#diff-589b240a6583248588fd7dd9ae6b3a9ce5f870b24ef806194a444ef5c1a960d4R42-R43) but I wasn't sure if it's somewhere we might want `uv` to diverge

---

_Comment by @charliermarsh on 2025-01-13 15:41_

Thanks @ericmarkmartin, will review!

---

_@charliermarsh approved on 2025-01-13 22:40_

Thanks! The main change I made is that I think `extends` could display strange behavior because `by_name` and `by_url` stores indexes into the distributions vector, so we'd have to do a bunch of fancy stuff to correct it. Instead, we now just iterate over the flattened distributions directly.

---

_Merged by @charliermarsh on 2025-01-13 22:50_

---

_Closed by @charliermarsh on 2025-01-13 22:50_

---
