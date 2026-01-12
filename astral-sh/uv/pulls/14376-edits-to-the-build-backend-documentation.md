```yaml
number: 14376
title: Edits to the build backend documentation
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/build-backend-docs
created_at: 2025-06-30T20:43:06Z
updated_at: 2025-07-01T08:44:24Z
url: https://github.com/astral-sh/uv/pull/14376
synced_at: 2026-01-12T16:11:11Z
```

# Edits to the build backend documentation

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2025-06-30 20:43_

---

_Review comment by @konstin on `docs/concepts/build-backend.md`:181 on 2025-06-30 21:26_

I consider the structure of a wheel file an implementation detail, hasit doesn't correspond to neither project nor installed structure (if you know the structure of a wheel file, then you don't need this guide, if you don't know how a wheel is installed, the location in the wheel doesn't tell you much). We could say where the data gets installed to, i.e. site packages,  dist info or respective data directories.

---

_@konstin reviewed on 2025-06-30 21:26_

---

_@zanieb reviewed on 2025-06-30 21:38_

---

_Review comment by @zanieb on `docs/concepts/build-backend.md`:181 on 2025-06-30 21:38_

We could just omit these then, I was just trying to match the existing text. I'm not sure it hurts though and it might set expectations for things like the README?

---

_@konstin reviewed on 2025-06-30 21:52_

---

_Review comment by @konstin on `docs/concepts/build-backend.md`:181 on 2025-06-30 21:52_

It would match both demands if we just removed the "which is moved to the root directory.", cause the other three items are already correct for both the wheels and the venv they are installed to.

---

_Marked ready for review by @zanieb on 2025-06-30 22:15_

---

_Review requested from @konstin by @zanieb on 2025-06-30 22:15_

---

_@konstin approved on 2025-07-01 08:33_

Thanks

---

_Merged by @konstin on 2025-07-01 08:44_

---

_Closed by @konstin on 2025-07-01 08:44_

---

_Branch deleted on 2025-07-01 08:44_

---
