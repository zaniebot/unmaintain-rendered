```yaml
number: 13108
title: "Remove `--version` from subcommands"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - breaking
assignees: []
merged: true
base: release/070
head: zb/version-flags
created_at: 2025-04-25T16:19:28Z
updated_at: 2025-04-28T15:43:32Z
url: https://github.com/astral-sh/uv/pull/13108
synced_at: 2026-01-10T11:10:40Z
```

# Remove `--version` from subcommands

---

_Pull request opened by @zanieb on 2025-04-25 16:19_

Supersedes https://github.com/astral-sh/uv/pull/12439 — does not use the Clap macro so we retain control over the messages
Closes #12431 

https://github.com/astral-sh/uv/pull/13108/commits/0077a67b34d22071dcfd42c9281c6929658bb7f9 pulls `uv run` and `uv tool run` test changes from https://github.com/astral-sh/uv/pull/12439

---

_Comment by @zanieb on 2025-04-25 16:38_

Weird, this fails during shell completion generation on Windows? Ah, we do something hacky to generate those completions.

---

_Label `breaking` added by @zanieb on 2025-04-25 16:58_

---

_Label `cli` added by @zanieb on 2025-04-25 16:58_

---

_Marked ready for review by @zanieb on 2025-04-25 16:58_

---

_Review requested from @Gankra by @zanieb on 2025-04-25 23:52_

---

_Review requested from @jtfmumm by @zanieb on 2025-04-25 23:52_

---

_@jtfmumm approved on 2025-04-26 15:48_

---

_Merged by @zanieb on 2025-04-28 15:43_

---

_Closed by @zanieb on 2025-04-28 15:43_

---

_Branch deleted on 2025-04-28 15:43_

---
