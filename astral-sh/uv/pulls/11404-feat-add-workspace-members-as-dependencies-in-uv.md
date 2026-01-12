```yaml
number: 11404
title: "feat: add workspace members as dependencies in uv init"
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: feature/5388-workspace-member-as-dependency
created_at: 2025-02-10T20:48:30Z
updated_at: 2025-09-11T22:52:38Z
url: https://github.com/astral-sh/uv/pull/11404
synced_at: 2026-01-12T16:09:50Z
```

# feat: add workspace members as dependencies in uv init

---

_@Aditya-PS-05_

closes #5388 
## Description
This PR enhances `uv init` to automatically add workspace members as dependencies in the root project's `pyproject.toml`. When initializing a new workspace member, it now:

1. Adds the member to `tool.uv.workspace.members`
2. Adds the member to `project.dependencies`
3. Configures the member in `tool.uv.sources` with `workspace = true`

## Changes
- Modified `init_project` to add the new member as a dependency
- Uses existing `add_dependency` method for consistency
- Maintains the same formatting and structure as `uv add`


---

_@zanieb reviewed on 2025-02-10 21:15_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:1996 on 2025-02-10 21:15_

Hm this is probably wrong, we shouldn't create a project table / add it in a virtual workspace.

---

_@zanieb reviewed on 2025-02-10 21:16_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:1212 on 2025-02-10 21:16_

I wonder if we should have an opt-out for this? It seems a little awkward since there's no version bound. What if you wanted it as a dev dependency? I wonder if we should even do this by default for these reasons?

---

_@zanieb reviewed on 2025-02-10 21:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:1212 on 2025-02-10 21:17_

In the issue I suggested a toggle and @charliermarsh disagreed, so perhaps we should revisit that conversation briefly?

---

_@zanieb reviewed on 2025-02-10 21:18_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:1212 on 2025-02-10 21:18_

(it seems more annoying to _undo_ this automatic add then to need t do it manually?)

---

_Comment by @Aditya-PS-05 on 2025-02-10 22:06_

@zanieb , 
So I need to remove the automatic dependency addition and else implementation is correct.

---

_Comment by @zanieb on 2025-02-10 22:14_

Well, we shouldn't add it as a dependency in virtual projects for sure.

The rest we'll need to discuss.

---

_Comment by @Aditya-PS-05 on 2025-08-24 17:38_

@zanieb , ?

---

_Closed by @Aditya-PS-05 on 2025-09-11 22:52_

---
