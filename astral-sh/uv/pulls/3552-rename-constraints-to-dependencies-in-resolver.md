```yaml
number: 3552
title: "Rename \"constraints\" to \"dependencies\" in resolver"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/editable-constraint
created_at: 2024-05-13T16:20:27Z
updated_at: 2024-05-13T16:30:17Z
url: https://github.com/astral-sh/uv/pull/3552
synced_at: 2026-01-10T14:37:54Z
```

# Rename "constraints" to "dependencies" in resolver

---

_Pull request opened by @charliermarsh on 2024-05-13 16:20_

## Summary

It's confusing that we use `constraints` here because constraints mean something else for us (e.g., `--constraint constraints.txt`). These are really the dependencies of a given `PubGrubPackage` -- the type is even called `PubGrubDependencies`.

---

_Label `internal` added by @charliermarsh on 2024-05-13 16:20_

---

_Marked ready for review by @charliermarsh on 2024-05-13 16:20_

---

_@konstin approved on 2024-05-13 16:20_

---

_Merged by @charliermarsh on 2024-05-13 16:30_

---

_Closed by @charliermarsh on 2024-05-13 16:30_

---

_Branch deleted on 2024-05-13 16:30_

---
