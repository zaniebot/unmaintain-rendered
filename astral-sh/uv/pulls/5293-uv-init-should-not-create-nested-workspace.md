```yaml
number: 5293
title: "`uv init` should not create nested workspace"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: init-nested
created_at: 2024-07-22T16:15:10Z
updated_at: 2024-07-23T14:37:23Z
url: https://github.com/astral-sh/uv/pull/5293
synced_at: 2026-01-12T16:06:44Z
```

# `uv init` should not create nested workspace

---

_@j178_

## Summary

Resolves #5251

---

_Review requested from @ibraheemdev by @zanieb on 2024-07-22 16:35_

---

_@idlsoft reviewed on 2024-07-22 19:56_

---

_Review comment by @idlsoft on `crates/uv-workspace/src/workspace.rs`:695 on 2024-07-22 19:56_

is this true for a virtual workspaces?

---

_Review comment by @j178 on `crates/uv-workspace/src/workspace.rs`:695 on 2024-07-22 21:17_

It is, every workspace has a root member, either explicit or implicit.

---

_@j178 reviewed on 2024-07-22 21:17_

---

_Marked ready for review by @j178 on 2024-07-22 21:25_

---

_@ibraheemdev reviewed on 2024-07-22 21:56_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/project/init.rs`:57 on 2024-07-22 21:56_

Hmm I'm not sure about backticks around the inner colored text. I don't think we have a consistent style-guide for this, but is the color not sufficient?

---

_@zanieb reviewed on 2024-07-22 21:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:57 on 2024-07-22 21:58_

We do, actually #5209 covers use of backticks in these cases. We want the output to be readable in colorless contexts too. 

---

_Comment by @charliermarsh on 2024-07-22 23:08_

I'm about to change this code so gonna tweak and merge this. I think this still doesn't work for virtual workspaces as-is.

---

_Assigned to @charliermarsh by @zanieb on 2024-07-22 23:16_

---

_@charliermarsh reviewed on 2024-07-22 23:40_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:66 on 2024-07-22 23:40_

@konstin - Any reason not to include this? It could be the same as the root member, but for virtual workspaces I can't see another way to get it.

---

_Merged by @charliermarsh on 2024-07-22 23:48_

---

_Closed by @charliermarsh on 2024-07-22 23:48_

---

_Branch deleted on 2024-07-23 14:37_

---
