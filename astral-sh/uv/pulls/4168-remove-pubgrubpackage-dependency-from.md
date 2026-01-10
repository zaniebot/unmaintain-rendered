```yaml
number: 4168
title: "Remove `PubGrubPackage` dependency from `ResolutionGraph`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
assignees: []
merged: true
base: main
head: charlie/markers-dep-ii
created_at: 2024-06-08T19:33:09Z
updated_at: 2024-06-10T13:41:41Z
url: https://github.com/astral-sh/uv/pull/4168
synced_at: 2026-01-10T13:54:02Z
```

# Remove `PubGrubPackage` dependency from `ResolutionGraph`

---

_Pull request opened by @charliermarsh on 2024-06-08 19:33_

## Summary

Similar to how we abstracted the dependencies into `ResolutionDependencyNames`, I think it makes sense to abstract the base packages into a `ResolutionPackage`. This also avoids leaking details about the various `PubGrubPackage` enum variants to `ResolutionGraph`.


---

_Label `internal` added by @charliermarsh on 2024-06-08 19:33_

---

_Label `preview` added by @charliermarsh on 2024-06-08 19:45_

---

_@konstin approved on 2024-06-10 06:38_

---

_Merged by @charliermarsh on 2024-06-10 12:50_

---

_Closed by @charliermarsh on 2024-06-10 12:50_

---

_Branch deleted on 2024-06-10 12:50_

---

_@BurntSushi reviewed on 2024-06-10 13:41_

Oh yes, this makes me very happy.

The next thought I have is whether we can collapse the case analysis that's based on whether `url` is `Some` or `None`.

---
