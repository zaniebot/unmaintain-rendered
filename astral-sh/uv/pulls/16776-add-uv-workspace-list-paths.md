```yaml
number: 16776
title: "Add `uv workspace list --paths`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/workspace-list-paths
created_at: 2025-11-19T15:40:37Z
updated_at: 2025-11-20T19:44:58Z
url: https://github.com/astral-sh/uv/pull/16776
synced_at: 2026-01-12T16:12:26Z
```

# Add `uv workspace list --paths`

---

_@zanieb_

I initially thought I didn't need this, but in some contexts, the workspace member name is not useful at all and I just want to iterate over the paths without composing with `uv workspace dir --package <name>`

---

_Label `preview` added by @zanieb on 2025-11-19 15:40_

---

_Renamed from "zb/workspace list paths" to "Add `uv workspace list --paths`" by @zanieb on 2025-11-19 15:45_

---

_Comment by @zanieb on 2025-11-19 15:47_

Otherwise, the code I'm replacing looks like

```diff
index b66739d4..b15e6d79 100644
--- a/.../runner.py
+++ b/.../runner.py
@@ -45,13 +45,21 @@
 
 def find_workspace_members() -> Generator[pathlib.Path]:
     """Yield the root directories of all workspace members with tests."""
-    # TODO(zanieb): Use `uv metadata` for workspace member discovery once that exists
-    for path in WORKSPACE_ROOT.glob("foo-*/pyproject.toml"):
-        member_root = path.parent
-        if not (member_root / "tests").is_dir():
-            continue
-
-        yield member_root
+    output = subprocess.check_output(
+        ["uv", "workspace", "list", "--preview"], cwd=WORKSPACE_ROOT
+    )
+    for member in output.decode().splitlines():
+        member_root = (
+            subprocess.check_output(
+                ["uv", "workspace", "dir", "--preview", "--package", member],
+                cwd=WORKSPACE_ROOT,
+            )
+            .decode()
+            .strip()
+        )
+        path = pathlib.Path(member_root)
+        if (path / "tests").is_dir():
+            yield path
 
```

---

_Review requested from @zsol by @zanieb on 2025-11-20 18:31_

---

_@zsol approved on 2025-11-20 19:34_

<a href="https://gitme.me/image?url=https%3A%2F%2Fmedia0.giphy.com%2Fmedia%2FNEvPzZ8bd1V4Y%2Fgiphy-downsized-medium.gif&token=nice" data-gitmeme-token="nice"><img src="https://media0.giphy.com/media/NEvPzZ8bd1V4Y/giphy-downsized-medium.gif" title="Created by gitme.me with /nice"
        alt="nice"/></a>

I was going to suggest listing both names and paths, but I realized paths are more powerful (one can always cd into it and look up the name).
Thoughts about changing the default to list paths and have a `--names` option instead?

---

_Comment by @zanieb on 2025-11-20 19:36_

You can also compose it the other direction, e.g., `uv workspace dir --package <name>`. I'm not sure one is inherently more powerful than the other. I find the names a little friendlier for the default output / consistent with the rest of uv.

---

_Merged by @zanieb on 2025-11-20 19:44_

---

_Closed by @zanieb on 2025-11-20 19:44_

---

_Branch deleted on 2025-11-20 19:44_

---
