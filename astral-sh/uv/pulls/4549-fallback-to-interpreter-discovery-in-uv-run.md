```yaml
number: 4549
title: "Fallback to interpreter discovery in `uv run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/isolated-run
created_at: 2024-06-26T14:26:57Z
updated_at: 2024-06-26T17:47:03Z
url: https://github.com/astral-sh/uv/pull/4549
synced_at: 2026-01-12T16:06:18Z
```

# Fallback to interpreter discovery in `uv run`

---

_@charliermarsh_

## Summary

This PR modifies `uv run` to fallback to discovering an interpreter (e.g., a local `.venv`) if the command is run outside of a workspace.

`uv run --isolated` continues to completely skip workspace _and_ interpreter discovering, only installing whatever's provided with `--with`.

The next step here is adding some ergonomic controls for enabling this behavior even if your project is technically in a workspace (i.e., you have a `pyproject.toml` but aren't using the Project APIs and don't want locking etc.). I could imagine a setting in `pyproject.toml` that's also exposed on the command-line. Something like: `managed = false` or `project = false`.

See: https://github.com/astral-sh/uv/issues/3836.


---

_Review requested from @zanieb by @charliermarsh on 2024-06-26 14:27_

---

_@charliermarsh reviewed on 2024-06-26 14:27_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:106 on 2024-06-26 14:27_

This is all just indented in `if let Some(project) = project` below.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 14:27_

I figured `find_or_fetch` makes sense here...?

---

_@charliermarsh reviewed on 2024-06-26 14:27_

---

_Label `preview` added by @charliermarsh on 2024-06-26 14:27_

---

_@zanieb reviewed on 2024-06-26 14:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 14:47_

We `find_or_fetch` lower down too. We should only fetch a toolchain if we want to create an environment. I'll need to look at the file outside the diff to see what makes sense.

---

_@charliermarsh reviewed on 2024-06-26 14:57_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 14:57_

We don't install anything into this environment so it may not matter.

---

_@zanieb reviewed on 2024-06-26 15:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 15:05_

If you're just looking for an environment to run a command in, `PythonEnvironment::find` is probably more appropriate? Later, if we need to install something, we need to create a new environment anyway? I'll check out the PR in a second though and read through.

---

_@zanieb reviewed on 2024-06-26 15:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 15:47_

I took a look at this and made the following change. However, this comes with some downsides as we now run interpreter discovery twice when we need to fetch a toolchain — this probably isn't worth it. I think what you have is good, just sharing what I looked into for additional context. We should probably do some refactoring after this because the `find_or_fetch` in ephemeral environment construction is now _only_ for the isolated case but that's not obvious.

```diff
diff --git a/crates/uv/src/commands/project/run.rs b/crates/uv/src/commands/project/run.rs
index 368f7478..da0f9dde 100644
--- a/crates/uv/src/commands/project/run.rs
+++ b/crates/uv/src/commands/project/run.rs
@@ -14,7 +14,8 @@ use uv_distribution::{ProjectWorkspace, Workspace, WorkspaceError};
 use uv_normalize::PackageName;
 use uv_requirements::RequirementsSource;
 use uv_toolchain::{
-    EnvironmentPreference, PythonEnvironment, Toolchain, ToolchainPreference, ToolchainRequest,
+    EnvironmentPreference, Error as ToolchainError, PythonEnvironment, Toolchain,
+    ToolchainPreference, ToolchainRequest,
 };
 use uv_warnings::warn_user;
 
@@ -68,7 +69,7 @@ pub(crate) async fn run(
             }
         };
 
-        let venv = if let Some(project) = project {
+        if let Some(project) = project {
             debug!(
                 "Discovered project `{}` at: {}",
                 project.project_name(),
@@ -117,27 +118,25 @@ pub(crate) async fn run(
             )
             .await?;
 
-            venv
+            Some(venv)
         } else {
             debug!("No project found; searching for Python interpreter");
 
-            let client_builder = BaseClientBuilder::new()
-                .connectivity(connectivity)
-                .native_tls(native_tls);
-
-            let toolchain = Toolchain::find_or_fetch(
-                python.as_deref().map(ToolchainRequest::parse),
+            match PythonEnvironment::find(
+                &python
+                    .as_deref()
+                    .map(ToolchainRequest::parse)
+                    .unwrap_or_default(),
+                // No opt-in is required for system environments, since we are not mutating it
                 EnvironmentPreference::Any,
-                toolchain_preference,
-                client_builder,
                 cache,
-            )
-            .await?;
-
-            PythonEnvironment::from_toolchain(toolchain)
-        };
-
-        Some(venv)
+            ) {
+                Ok(env) => Some(env),
+                // If no environment is found, we'll create an environment later
+                Err(ToolchainError::NotFound(_)) => None,
+                Err(err) => return Err(err.into()),
+            }
+        }
     };
 
     if let Some(project_env) = &project_env {
```


---

_@zanieb reviewed on 2024-06-26 15:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:131 on 2024-06-26 15:47_

```suggestion
                // No opt-in is required for system environments, since we are not mutating it
                EnvironmentPreference::Any,
```

---

_@zanieb reviewed on 2024-06-26 15:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:143 on 2024-06-26 15:48_

It might make sense to rename `project_env` to something like `base_env` now that it can come from the system?

---

_@zanieb reviewed on 2024-06-26 15:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 15:50_

Another note... technically it is a no-no to construct a `PythonEnvironment` from a `find_or_fetch` call because you might accidentally end up allowing mutation of a managed toolchain environment. Since we're not mutating, it's fine here — but suggests that something isn't quite right in the structure (but the problem could be in the toolchain APIs themselves).

---

_@zanieb approved on 2024-06-26 15:50_

---

_@charliermarsh reviewed on 2024-06-26 16:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 16:09_

For reference, we use the `PythonEnvironment` for two things: (1) `interpreter.scripts()`, and (2) site-packages.

---

_Merged by @charliermarsh on 2024-06-26 16:25_

---

_Closed by @charliermarsh on 2024-06-26 16:25_

---

_Branch deleted on 2024-06-26 16:25_

---

_@zanieb reviewed on 2024-06-26 16:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 16:28_

These are both attributes of the `Interpreter` and don't require a `PythonEnvironment` type, right?

---

_@charliermarsh reviewed on 2024-06-26 17:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 17:41_

Assuming `target` and `prefix` is disabled, yes...

---

_@charliermarsh reviewed on 2024-06-26 17:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:136 on 2024-06-26 17:47_

https://github.com/astral-sh/uv/pull/4559

---
