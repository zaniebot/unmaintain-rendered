```yaml
number: 10817
title: "Update docs on how to use `UV_PROJECT_ENVIRONMENT` to use the system python environment"
type: pull_request
state: merged
author: NiklasRosenstein
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-01-21T15:51:20Z
updated_at: 2025-01-22T03:40:28Z
url: https://github.com/astral-sh/uv/pull/10817
synced_at: 2026-01-12T16:09:30Z
```

# Update docs on how to use `UV_PROJECT_ENVIRONMENT` to use the system python environment

---

_@NiklasRosenstein_

## Summary

The docs did mention that you could set the `UV_PROJECT_ENVIRONMENT` variable to point Uv to use the system Python environment (e.g. for use in CI or Docker), but it did not document _how_.

Reference: https://github.com/astral-sh/uv/pull/6834#issuecomment-2319253359



---

_Review comment by @zanieb on `docs/concepts/projects/config.md`:174 on 2025-01-21 15:53_

This is specific to your system, this path differs depending on the OS

---

_@zanieb reviewed on 2025-01-21 15:53_

---

_@NiklasRosenstein reviewed on 2025-01-21 16:03_

---

_Review comment by @NiklasRosenstein on `docs/concepts/projects/config.md`:174 on 2025-01-21 16:03_

Forgive my ignorance, I assumed this would be accurate enough in most practical cases.

How about I change it to clarify that this as an example?

> For example, on a Debian-based OS, you can use `UV_PROJECT_ENVIRONMENT=/usr/local`.


---

_Review comment by @zanieb on `docs/concepts/projects/config.md`:174 on 2025-01-21 16:06_

That sounds great, thank you!

---

_@zanieb reviewed on 2025-01-21 16:06_

---

_@NiklasRosenstein reviewed on 2025-01-21 16:08_

---

_Review comment by @NiklasRosenstein on `docs/concepts/projects/config.md`:174 on 2025-01-21 16:08_

Chose a slightly different wording, please check again!

---

_Assigned to @zanieb by @zanieb on 2025-01-21 16:15_

---

_@zanieb approved on 2025-01-21 18:33_

---

_Label `documentation` added by @zanieb on 2025-01-21 18:33_

---

_Merged by @zanieb on 2025-01-21 18:40_

---

_Closed by @zanieb on 2025-01-21 18:40_

---

_Branch deleted on 2025-01-22 03:37_

---

_Comment by @NiklasRosenstein on 2025-01-22 03:38_

Thanks @zanieb, your edit is much better 🙂 

---

_Comment by @zanieb on 2025-01-22 03:40_

haha thanks :) appreciate that you took the time to contribute

---
