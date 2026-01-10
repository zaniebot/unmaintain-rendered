```yaml
number: 8573
title: Bump version to 0.4.27
type: pull_request
state: merged
author: zanieb
labels:
  - no-build
  - releases
assignees: []
merged: true
base: main
head: zb/0427
created_at: 2024-10-25T18:33:29Z
updated_at: 2024-10-25T19:06:07Z
url: https://github.com/astral-sh/uv/pull/8573
synced_at: 2026-01-10T12:54:12Z
```

# Bump version to 0.4.27

---

_Pull request opened by @zanieb on 2024-10-25 18:33_

_No description provided._

---

_Label `releases` added by @zanieb on 2024-10-25 18:33_

---

_@charliermarsh reviewed on 2024-10-25 18:35_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:5 on 2024-10-25 18:35_

"of optional dependency groups" -- should we avoid "optional"?

---

_@charliermarsh reviewed on 2024-10-25 18:35_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:5 on 2024-10-25 18:35_

Instead of "options throughout the uv interface."... "flags throughout" to avoid overloading "options"?

---

_@charliermarsh reviewed on 2024-10-25 18:36_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:9 on 2024-10-25 18:36_

"and an automatic migration to ease the transition." -- I might omit this, what if it never ships?

---

_@charliermarsh approved on 2024-10-25 18:36_

---

_@zanieb reviewed on 2024-10-25 18:38_

---

_Review comment by @zanieb on `CHANGELOG.md`:5 on 2024-10-25 18:38_

I felt it was important here and okay since I am comparing to `project.optional-dependencies` in the same sentence.

---

_@zanieb reviewed on 2024-10-25 18:41_

---

_Review comment by @zanieb on `CHANGELOG.md`:5 on 2024-10-25 18:41_

I use "flags" for boolean CLI components that do not take values and "options" for CLI components that take values. I try to be consistent about this in the documentation. I'm hesitant to switch here, but don't feel strongly if you found it confusing as-is.

---

_@zanieb reviewed on 2024-10-25 18:42_

---

_Review comment by @zanieb on `CHANGELOG.md`:9 on 2024-10-25 18:42_

I considered omitting it too haha but I really want to ship this. Hm.. I'll remove it.

---

_Label `no-build` added by @zanieb on 2024-10-25 18:56_

---

_Merged by @zanieb on 2024-10-25 19:06_

---

_Closed by @zanieb on 2024-10-25 19:06_

---

_Branch deleted on 2024-10-25 19:06_

---
