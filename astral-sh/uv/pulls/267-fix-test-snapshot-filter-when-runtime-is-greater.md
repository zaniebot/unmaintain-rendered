```yaml
number: 267
title: Fix test snapshot filter when runtime is greater than 1s
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/test-sec
created_at: 2023-11-01T04:47:17Z
updated_at: 2023-11-01T13:15:07Z
url: https://github.com/astral-sh/uv/pull/267
synced_at: 2026-01-10T15:50:28Z
```

# Fix test snapshot filter when runtime is greater than 1s

---

_Pull request opened by @zanieb on 2023-11-01 04:47_

Tests would sometimes flake with this locally e.g. "1.50s" was not filtered correctly.

Verified with

```diff
diff --git a/crates/puffin-cli/src/commands/pip_compile.rs b/crates/puffin-cli/src/commands/pip_compile.rs
index 0193216..2d6f8af 100644
--- a/crates/puffin-cli/src/commands/pip_compile.rs
+++ b/crates/puffin-cli/src/commands/pip_compile.rs
@@ -150,6 +150,8 @@ pub(crate) async fn pip_compile(
         result => result,
     }?;
 
+    std::thread::sleep(std::time::Duration::from_secs(1));
+
     let s = if resolution.len() == 1 { "" } else { "s" };
     writeln!(
         printer,
```


---

_@charliermarsh approved on 2023-11-01 04:55_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:76 on 2023-11-01 12:15_

Can we put the vec into a const or static? I checked the macros and it doesn't match on the filter expression contents

---

_@konstin approved on 2023-11-01 12:15_

---

_Comment by @charliermarsh on 2023-11-01 13:13_

Gonna merge because I'm running into this :joy:

---

_Merged by @charliermarsh on 2023-11-01 13:15_

---

_Closed by @charliermarsh on 2023-11-01 13:15_

---

_Branch deleted on 2023-11-01 13:15_

---
