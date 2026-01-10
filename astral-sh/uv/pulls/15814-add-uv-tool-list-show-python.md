```yaml
number: 15814
title: Add uv tool list --show-python
type: pull_request
state: merged
author: harshithvh
labels: []
assignees: []
merged: true
base: main
head: hvh/python-list-option
created_at: 2025-09-12T15:19:42Z
updated_at: 2025-10-10T17:33:27Z
url: https://github.com/astral-sh/uv/pull/15814
synced_at: 2026-01-10T06:36:15Z
```

# Add uv tool list --show-python

---

_Pull request opened by @harshithvh on 2025-09-12 15:19_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Closes #15312 
Closes https://github.com/astral-sh/uv/issues/16237

---

_Comment by @harshithvh on 2025-09-16 14:04_

@zanieb, it would be great if you could review this when you have some time
Thanks!

---

_@zanieb reviewed on 2025-09-16 14:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:118 on 2025-09-16 14:41_

per https://github.com/astral-sh/uv/pull/15312#discussion_r2279928761 I think we either want

`[python: <version>]`

or

`[<implementation> <version>]`

---

_@zanieb reviewed on 2025-09-16 14:41_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:118 on 2025-09-16 14:41_

I think the latter seems better

---

_@zanieb reviewed on 2025-09-16 14:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:64 on 2025-09-16 14:43_

Do we retain this somewhere? Should it be up in the `get_environment` call match?

---

_@zanieb reviewed on 2025-09-16 14:43_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:73 on 2025-09-16 14:43_

Are we using the Deref implementation? It makes less sense now that the type includes the package name too.

---

_@zanieb reviewed on 2025-09-16 14:43_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:57 on 2025-09-16 14:43_

This should be `into_environment`

---

_@zanieb reviewed on 2025-09-16 14:44_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:342 on 2025-09-16 14:44_

Can / should we drop this function and have callers use the `get_environment` pattern instead?

---

_@zanieb reviewed on 2025-09-16 14:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4584 on 2025-09-16 14:45_

```suggestion
    /// Whether to display the Python version associated with each tool.
```

---

_@harshithvh reviewed on 2025-09-19 05:13_

---

_Review comment by @harshithvh on `crates/uv-tool/src/lib.rs`:73 on 2025-09-19 05:13_

right, the Deref implementation does feel out of place now that `ToolEnvironment` is  a transparent wrapper around `PythonEnvironment` let me remove it and update the call sites

---

_@harshithvh reviewed on 2025-09-19 05:23_

---

_Review comment by @harshithvh on `crates/uv-tool/src/lib.rs`:342 on 2025-09-19 05:23_

makes sense, this looks redundant, all callers are already using `get_environment`

---

_@harshithvh reviewed on 2025-09-19 05:33_

---

_Review comment by @harshithvh on `crates/uv/src/commands/tool/list.rs`:64 on 2025-09-19 05:33_

retaining it here

---

_Comment by @harshithvh on 2025-09-19 07:12_

@zanieb all looks good to me
Mind giving a final review

---

_@zanieb approved on 2025-10-10 17:33_

---

_Merged by @zanieb on 2025-10-10 17:33_

---

_Closed by @zanieb on 2025-10-10 17:33_

---
