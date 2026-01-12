```yaml
number: 13109
title: "Fix display name for `uvx --version`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: release/070
head: zb/uvx-version-fix
created_at: 2025-04-25T16:34:22Z
updated_at: 2025-04-28T17:33:02Z
url: https://github.com/astral-sh/uv/pull/13109
synced_at: 2026-01-12T16:10:32Z
```

# Fix display name for `uvx --version`

---

_@zanieb_

Based on #13108 because I don't want to deal with rebasing conflicts across `main` and `release/070`.

```
# Before
❯ .uvx --version
uv-tool-uvx 0.6.16+23 (33b8b7340 2025-04-25)

# After
❯ uvx --version
uvx 0.6.16+23 (33b8b7340 2025-04-25)
```

For posterity, chased this down via https://github.com/clap-rs/clap/pull/3693 and https://github.com/clap-rs/clap/issues/1382

---

_Label `bug` added by @zanieb on 2025-04-25 16:36_

---

_Review requested from @Gankra by @zanieb on 2025-04-25 23:52_

---

_Review requested from @jtfmumm by @zanieb on 2025-04-25 23:52_

---

_@charliermarsh approved on 2025-04-26 14:09_

---

_Closed by @zanieb on 2025-04-28 15:56_

---

_Reopened by @zanieb on 2025-04-28 15:56_

---

_Merged by @zanieb on 2025-04-28 16:19_

---

_Closed by @zanieb on 2025-04-28 16:19_

---

_Branch deleted on 2025-04-28 16:19_

---

_Comment by @Gankra on 2025-04-28 17:33_

nice catch

---
