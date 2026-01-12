```yaml
number: 17032
title: "Move test support files out of `scripts/` into `test/`\n"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/test-support
created_at: 2025-12-08T15:44:26Z
updated_at: 2025-12-09T16:06:07Z
url: https://github.com/astral-sh/uv/pull/17032
synced_at: 2026-01-12T16:12:34Z
```

# Move test support files out of `scripts/` into `test/`


---

_@zanieb_

It's been bothering me that we have a bunch of stub packages and such in a `scripts` directory.

---

_Converted to draft by @zanieb on 2025-12-08 15:44_

---

_Marked ready for review by @zanieb on 2025-12-08 17:11_

---

_Review requested from @konstin by @zanieb on 2025-12-08 17:11_

---

_@konstin reviewed on 2025-12-09 13:31_

---

_Review comment by @konstin on `ruff.toml`:6 on 2025-12-09 13:31_

The ecosystem tests and the workspaces aren't packages like the other subdirectories of `test/packages/`, I'd put them into top-level `test/`.

---

_@zanieb reviewed on 2025-12-09 14:06_

---

_Review comment by @zanieb on `ruff.toml`:6 on 2025-12-09 14:06_

I'm not sure how strongly I agree and it's a lot of work to move them again.. but okay.

---

_Merged by @zanieb on 2025-12-09 16:06_

---

_Closed by @zanieb on 2025-12-09 16:06_

---

_Branch deleted on 2025-12-09 16:06_

---
