---
number: 17002
title: Cannot use uv sync with a workspace when project have an extra defined
type: issue
state: open
author: jabbera
labels:
  - error messages
assignees: []
created_at: 2025-12-05T17:45:23Z
updated_at: 2025-12-09T23:39:07Z
url: https://github.com/astral-sh/uv/issues/17002
synced_at: 2026-01-07T13:12:19-06:00
---

# Cannot use uv sync with a workspace when project have an extra defined

---

_Issue opened by @jabbera on 2025-12-05 17:45_

### Summary

When you have 2 packages in a workspace that has one that depends on the other and one has an extra defined, uv sync is failing with the message:

No solution found when resolving dependencies:
  ╰─▶ Because lib-b depends on lib-a and your workspace requires lib-a[dev], we can conclude that your workspace's
      requirements and lib-b are incompatible.
      And because your workspace requires lib-b, we can conclude that your workspace's requirements are unsatisfiable.

See repro repo here: https://github.com/[jabbera/uv_workspace_bug](https://github.com/jabbera/uv_workspace_bug)

git clone https://github.com/[jabbera/uv_workspace_bug](https://github.com/jabbera/uv_workspace_bug)
cd uv_workspace_bug/workspace
uv sync


### Platform

Linux

### Version

0.9.15

### Python version

_No response_

---

_Label `bug` added by @jabbera on 2025-12-05 17:45_

---

_Comment by @charliermarsh on 2025-12-05 17:49_

I think this is just a bad error message -- uv is correctly failing. Your `lib-b` depends on `dependencies = ["lib_a>=1.0.0"]`, but `lib-a` has `version = "0.1.0"`. You can fix it with:

```
diff --git a/packages/lib_a/pyproject.toml b/packages/lib_a/pyproject.toml
index 70b95d7..2d627ab 100644
--- a/packages/lib_a/pyproject.toml
+++ b/packages/lib_a/pyproject.toml
@@ -1,6 +1,6 @@
 [project]
 name = "lib-a"
-version = "0.1.0"
+version = "1.0.0"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.13"
```

I suspect we're accidentally removing (in an attempt to simplify) important information from the error message.


---

_Label `bug` removed by @charliermarsh on 2025-12-05 17:49_

---

_Label `error messages` added by @charliermarsh on 2025-12-05 17:49_

---

_Comment by @jabbera on 2025-12-05 17:57_

Indeed that did it. We replace the version field dynamically at build time in our environment so it's always 0.1a0 in our source tree adding:

override-dependencies = ["lib_a", "lib_b"] to the project file sorted it. Thank you very much.

---
