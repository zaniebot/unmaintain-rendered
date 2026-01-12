```yaml
number: 3411
title: Autofix PIE810 rule violations
type: pull_request
state: merged
author: kyoto7250
labels:
  - fixes
assignees: []
merged: true
base: main
head: pie810
created_at: 2023-03-09T01:30:56Z
updated_at: 2023-03-10T05:17:23Z
url: https://github.com/astral-sh/ruff/pull/3411
synced_at: 2026-01-12T04:39:44Z
```

# Autofix PIE810 rule violations

---

_Pull request opened by @kyoto7250 on 2023-03-09 01:30_

close #3360 

This PR implements the autofix for PIE810.

---

_Label `autofix` added by @charliermarsh on 2023-03-09 02:57_

---

_Comment by @charliermarsh on 2023-03-10 05:06_

Great PR!

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pie/rules.rs`:507 on 2023-03-10 05:07_

I tweaked this slightly to ensure that the combined `call` was inserted at the site of the _first_ duplicate index. Previously, it was always inserted at the front of the expression, which led to an unintuitive fix like this:

```diff
diff --git a/setup.py b/setup.py
index 85a2b26357..f20e219ad4 100644
--- a/setup.py
+++ b/setup.py
@@ -236,9 +236,7 @@ def is_macosx_sdk_path(path):
     """
     Returns True if 'path' can be located in a macOS SDK
     """
-    return ( (path.startswith('/usr/') and not path.startswith('/usr/local'))
-                or path.startswith('/System/Library')
-                or path.startswith('/System/iOSSupport') )
+    return ( path.startswith(('/System/Library', '/System/iOSSupport')) or path.startswith('/usr/') and not path.startswith('/usr/local') )
```

---

_@charliermarsh reviewed on 2023-03-10 05:07_

---

_Comment by @charliermarsh on 2023-03-10 05:13_

Thank you! Hope to see you in more PRs :)

---

_Merged by @charliermarsh on 2023-03-10 05:17_

---

_Closed by @charliermarsh on 2023-03-10 05:17_

---
