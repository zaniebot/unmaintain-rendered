```yaml
number: 5819
title: Use indent size of 4 for documentation
type: pull_request
state: closed
author: zanieb
labels:
  - documentation
assignees: []
draft: true
base: main
head: zb/indent-4
created_at: 2024-08-06T16:53:07Z
updated_at: 2024-08-15T19:35:50Z
url: https://github.com/astral-sh/uv/pull/5819
synced_at: 2026-01-10T13:09:50Z
```

# Use indent size of 4 for documentation

---

_Pull request opened by @zanieb on 2024-08-06 16:53_

This is required for proper sub-bullet rendering in mkdocs


---

_Label `documentation` added by @zanieb on 2024-08-06 16:53_

---

_@zanieb reviewed on 2024-08-06 16:57_

---

_Review comment by @zanieb on `PIP_COMPATIBILITY.md`:368 on 2024-08-06 16:57_

I cannot comprehend why this is changed.

---

_@zanieb reviewed on 2024-08-06 16:58_

---

_Review comment by @zanieb on `PIP_COMPATIBILITY.md`:368 on 2024-08-06 16:58_

It renders fine though.

---

_@zanieb reviewed on 2024-08-06 17:02_

---

_Review comment by @zanieb on `PIP_COMPATIBILITY.md`:368 on 2024-08-06 17:02_

Tragic:

- https://github.com/prettier/prettier/pull/15526
- https://github.com/prettier/prettier/issues/5019

---

_Comment by @zanieb on 2024-08-06 17:03_

It's horrible, but I don't know what else to do if we want to use prettier.

---

_Comment by @zanieb on 2024-08-06 17:05_

Going to use `<!-- prettier-ignore -->` for nested lists for now instead. Please watch out for this footgun.

---

_Converted to draft by @zanieb on 2024-08-06 17:09_

---

_Closed by @zanieb on 2024-08-06 18:22_

---

_Comment by @glenn-jocher on 2024-08-15 19:35_

I'm waiting for https://github.com/ultralytics/ultralytics/pull/15619 to merge, but it's taking ages unfortunately.

---
