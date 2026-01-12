```yaml
number: 8688
title: "Clarify relationship between specifiers and `requires-python` range"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-10-29T21:21:32Z
updated_at: 2024-10-29T21:30:03Z
url: https://github.com/astral-sh/uv/pull/8688
synced_at: 2026-01-12T16:08:27Z
```

# Clarify relationship between specifiers and `requires-python` range

---

_@charliermarsh_

## Summary

Following up on a conversation with @konstin from Discord.


---

_Review requested from @konstin by @charliermarsh on 2024-10-29 21:21_

---

_Label `documentation` added by @charliermarsh on 2024-10-29 21:21_

---

_Comment by @konstin on 2024-10-29 21:29_

To add export more context from the conversation: The specifiers are not used in the resolver itself, they are used e.g. for filling the `requires-python` field in a new package or script. The `range` on the other hand is only used in the resolver, so these types are used separately in separate phases of the program and are allowed to become disjoint. We could probably separate the type if we wanted, so that there's a `VersionSpecifiers` initially for initializing packages and scripts and a separate `RequiresPythonRange` that's used in the resolver and for filtering wheels.

---

_@konstin approved on 2024-10-29 21:29_

---

_Merged by @konstin on 2024-10-29 21:30_

---

_Closed by @konstin on 2024-10-29 21:30_

---

_Branch deleted on 2024-10-29 21:30_

---
