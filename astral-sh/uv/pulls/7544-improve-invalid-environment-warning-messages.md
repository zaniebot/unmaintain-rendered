```yaml
number: 7544
title: Improve invalid environment warning messages
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/sync-invalid
created_at: 2024-09-19T11:51:02Z
updated_at: 2024-09-19T17:50:45Z
url: https://github.com/astral-sh/uv/pull/7544
synced_at: 2026-01-10T12:53:49Z
```

# Improve invalid environment warning messages

---

_Pull request opened by @zanieb on 2024-09-19 11:51_

Adds display of the target path of the link (since the link filename itself is basically static) and distinguishes between broken links and missing files.

---

_Label `cli` added by @zanieb on 2024-09-19 11:51_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-19 12:08_

---

_Review requested from @lucab by @zanieb on 2024-09-19 12:15_

---

_Review comment by @lucab on `crates/uv-tool/src/lib.rs`:217 on 2024-09-19 12:47_

Side note: there is a FS race here, as the symlink check and read are not atomic and the underlying FS may change in-between (TOCTOU). Probably not much relevant in this context, but consider whether you'd like to turn the error case into a `.unwrap_or(interpreter_path)` so that the execution flow is guaranteed to continue past here in all cases.

---

_@lucab approved on 2024-09-19 12:48_

LGTM with a side comment for your consideration.

---

_Merged by @zanieb on 2024-09-19 14:11_

---

_Closed by @zanieb on 2024-09-19 14:11_

---

_Branch deleted on 2024-09-19 14:11_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:217 on 2024-09-19 17:29_

Should this be an unconditional read_link with appropriate error handling for non-symlinks?

---

_@charliermarsh reviewed on 2024-09-19 17:29_

---

_@zanieb reviewed on 2024-09-19 17:42_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:217 on 2024-09-19 17:42_

That'd be a nice way to avoid the problem — if the IO error has the relevant details.

---

_@zanieb reviewed on 2024-09-19 17:49_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:217 on 2024-09-19 17:49_

It looks like this would still require two checks

```diff
diff --git a/crates/uv/src/commands/project/mod.rs b/crates/uv/src/commands/project/mod.rs
index 5324ac9e3..1300f0ae5 100644
--- a/crates/uv/src/commands/project/mod.rs
+++ b/crates/uv/src/commands/project/mod.rs
@@ -401,18 +401,14 @@ impl FoundInterpreter {
                 }
             }
             Err(uv_python::Error::Query(uv_python::InterpreterError::NotFound(path))) => {
-                if path.is_symlink() {
-                    let target_path = fs_err::read_link(&path)?;
-                    warn_user!(
-                        "Ignoring existing virtual environment linked to non-existent Python interpreter: {} -> {}",
-                        path.user_display().cyan(),
-                        target_path.user_display().cyan(),
-                    );
-                } else {
-                    warn_user!(
-                        "Ignoring existing virtual environment with missing Python interpreter: {}",
-                        path.user_display().cyan()
-                    );
+                match fs_err::read_link(&path) {
+                    Ok(path) => {
+                        dbg!(&path);
+                        dbg!(path.exists());
+                    }
+                    Err(err) => {
+                        dbg!(err);
+                    }
                 }
             }
             Err(err) => return Err(err.into()),
diff --git a/crates/uv/tests/sync.rs b/crates/uv/tests/sync.rs
index 95ec51743..c40009e8c 100644
--- a/crates/uv/tests/sync.rs
+++ b/crates/uv/tests/sync.rs
@@ -2687,7 +2687,8 @@ fn sync_invalid_environment() -> Result<()> {
         ----- stdout -----
 
         ----- stderr -----
-        warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/[BIN]/python -> python
+        [crates/uv/src/commands/project/mod.rs:406:25] &path = "python"
+        [crates/uv/src/commands/project/mod.rs:407:25] path.exists() = false
         Using Python 3.12.[X] interpreter at: [PYTHON-3.12]
         Removed virtual environment at: .venv
         Creating virtual environment at: .venv
@@ -2705,7 +2706,18 @@ fn sync_invalid_environment() -> Result<()> {
     ----- stdout -----
 
     ----- stderr -----
-    warning: Ignoring existing virtual environment with missing Python interpreter: .venv/[BIN]/python
+    [crates/uv/src/commands/project/mod.rs:410:25] err = Custom {
+        kind: NotFound,
+        error: Error {
+            kind: ReadLink,
+            source: Os {
+                code: 2,
+                kind: NotFound,
+                message: "No such file or directory",
+            },
+            path: "[VENV]/[BIN]/python",
+        },
+    }
     Using Python 3.12.[X] interpreter at: [PYTHON-3.12]
     Removed virtual environment at: .venv
     Creating virtual environment at: .venv
```

---

_@zanieb reviewed on 2024-09-19 17:50_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:217 on 2024-09-19 17:50_

And if you try to match on `canonicalize` the error is identical in both cases.

---
