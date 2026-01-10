```yaml
number: 3365
title: "Fix Git URL construction in `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/source
created_at: 2024-05-04T02:54:05Z
updated_at: 2024-05-04T03:02:15Z
url: https://github.com/astral-sh/uv/pull/3365
synced_at: 2026-01-10T14:37:54Z
```

# Fix Git URL construction in `tool.uv.sources`

---

_Pull request opened by @charliermarsh on 2024-05-04 02:54_

## Summary

We were including the `git+` prefix twice:

```
DEBUG At least one requirement is not satisfied: boltons @ git+git+https://github.com/mahmoud/boltons@57fbaa9b673ed85b32458b31baeeae230520e4a0@57fbaa9b673ed85b32458b31baeeae230520e4a0
```

## Test Plan

Extended the test to include a Git URL; verified that we don't trigger a reinstall.


---

_Label `bug` added by @charliermarsh on 2024-05-04 02:54_

---

_Marked ready for review by @charliermarsh on 2024-05-04 02:54_

---

_@charliermarsh reviewed on 2024-05-04 02:57_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:452 on 2024-05-04 02:57_

@konstin - There isn't really any point in reconstructing these, I don't think? It's just gonna be the same as the serialized form of the URL. The purpose of `given` is to let us preserve environment variables (as an example) and other details that won't match the serialized representation.

---

_Merged by @charliermarsh on 2024-05-04 03:02_

---

_Closed by @charliermarsh on 2024-05-04 03:02_

---

_Branch deleted on 2024-05-04 03:02_

---
