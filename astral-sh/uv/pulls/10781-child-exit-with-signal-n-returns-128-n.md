```yaml
number: 10781
title: Child exit with signal n returns 128+n 
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/exit-status-on-signal
created_at: 2025-01-20T15:13:59Z
updated_at: 2025-01-24T15:20:34Z
url: https://github.com/astral-sh/uv/pull/10781
synced_at: 2026-01-12T16:09:29Z
```

# Child exit with signal n returns 128+n 

---

_@konstin_

Following https://tldp.org/LDP/abs/html/exitcodes.html, a fatal signal n gets the exit code 128+n (unix only).

Fixes https://github.com/astral-sh/uv/issues/10751

First commit is only deduplicating code without features changes, the second commit is the actual fix.

---

_Label `bug` added by @konstin on 2025-01-20 15:13_

---

_Review requested from @Gankra by @charliermarsh on 2025-01-21 00:28_

---

_Comment by @charliermarsh on 2025-01-21 00:29_

Tagging in @Gankra, I've never seen this before.

---

_Review comment by @Gankra on `crates/uv/src/commands/run.rs`:70 on 2025-01-21 18:38_

oh hey this is exactly the platform-specific code that I was surprised to see when looking at #9652, still not sure why we don't do an equivalent thing on windows...

---

_@Gankra approved on 2025-01-21 18:38_

Ah yep this looks familiar

---

_@konstin reviewed on 2025-01-21 18:41_

---

_Review comment by @konstin on `crates/uv/src/commands/run.rs`:70 on 2025-01-21 18:41_

I'm not familiar with how signals work on windows, what do we need to do for windows?

---

_@Gankra reviewed on 2025-01-21 19:35_

---

_Review comment by @Gankra on `crates/uv/src/commands/run.rs`:70 on 2025-01-21 19:35_

I suppose it's more of a question of: why do we need to explicitly kill the script? Won't killing our own process "normally" in response to CTRL+C achieve the desired effect since it's a child process?

---

_@zanieb reviewed on 2025-01-21 19:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:70 on 2025-01-21 19:47_

Is the context you're looking for in

- https://github.com/astral-sh/uv/pull/8933
- https://github.com/astral-sh/uv/pull/6738

---

_@Gankra reviewed on 2025-01-24 15:04_

---

_Review comment by @Gankra on `crates/uv/src/commands/run.rs`:70 on 2025-01-24 15:04_

Wow yep it sure does, thanks!

---

_Merged by @konstin on 2025-01-24 15:20_

---

_Closed by @konstin on 2025-01-24 15:20_

---

_Branch deleted on 2025-01-24 15:20_

---
