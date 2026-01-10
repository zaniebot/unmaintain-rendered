```yaml
number: 13925
title: Use TTY detection to determine if SIGINT forwarding is enabled
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/sigint-tty
created_at: 2025-06-09T16:43:37Z
updated_at: 2025-06-11T13:28:35Z
url: https://github.com/astral-sh/uv/pull/13925
synced_at: 2026-01-10T11:10:42Z
```

# Use TTY detection to determine if SIGINT forwarding is enabled

---

_Pull request opened by @zanieb on 2025-06-09 16:43_

Use TTY detection to determine when we should forward SIGINT instead of counting signals, which can lead to various problems where multiple SIGINTs are sent to a child after the first signal. Counting does not make sense in interactive situations that do not exit on interrupt, e.g., the Python REPL.

Closes https://github.com/astral-sh/uv/issues/13919
Closes https://github.com/astral-sh/uv/issues/12108

---

_Label `enhancement` added by @zanieb on 2025-06-09 16:43_

---

_Assigned to @oconnor663 by @zanieb on 2025-06-09 17:22_

---

_Review requested from @oconnor663 by @zanieb on 2025-06-09 17:22_

---

_Comment by @zanieb on 2025-06-09 17:23_

I was originally opposed to using this as a heuristic, but I think I've seen a few cases now that justify it.

---

_Marked ready for review by @zanieb on 2025-06-09 17:32_

---

_Review comment by @oconnor663 on `crates/uv/src/commands/run.rs`:158 on 2025-06-11 03:23_

This comment needs to be tweaked.

---

_@oconnor663 approved on 2025-06-11 03:24_

I spent too long tracking down the underlying cause of this weird behavior, and the result was https://github.com/astral-sh/python-build-standalone/pull/652. This looks like a reasonable change whether or not we take that patch in `python-build-standalone`.

---

_@zanieb reviewed on 2025-06-11 13:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:158 on 2025-06-11 13:19_

Thanks!

---

_Merged by @zanieb on 2025-06-11 13:28_

---

_Closed by @zanieb on 2025-06-11 13:28_

---

_Branch deleted on 2025-06-11 13:28_

---
