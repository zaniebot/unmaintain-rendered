```yaml
number: 4041
title: "Use union of `requires-python` in workspace"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/version-union
created_at: 2024-06-05T10:20:44Z
updated_at: 2024-06-06T19:21:03Z
url: https://github.com/astral-sh/uv/pull/4041
synced_at: 2026-01-10T13:54:02Z
```

# Use union of `requires-python` in workspace

---

_Pull request opened by @konstin on 2024-06-05 10:20_

Follow-up to #4016.

This exposes `Range` and `PubGrubSpecifier` from outside the resolver to use pubgrub's union creating a dependency edge we don't really want.

---

_Label `preview` added by @konstin on 2024-06-05 10:20_

---

_Marked ready for review by @konstin on 2024-06-06 14:31_

---

_@charliermarsh approved on 2024-06-06 19:17_

---

_Comment by @charliermarsh on 2024-06-06 19:18_

@konstin -- let's look for a way to remove exposing PubGrub here... I can try it.

---

_Merged by @charliermarsh on 2024-06-06 19:21_

---

_Closed by @charliermarsh on 2024-06-06 19:21_

---

_Branch deleted on 2024-06-06 19:21_

---
