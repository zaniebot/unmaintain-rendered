```yaml
number: 16815
title: "Clarify 'uv python upgrade' message when no versions exist"
type: pull_request
state: open
author: sinsniwal
labels:
  - error messages
assignees: []
base: main
head: main
created_at: 2025-11-21T22:25:36Z
updated_at: 2025-11-28T02:26:23Z
url: https://github.com/astral-sh/uv/pull/16815
synced_at: 2026-01-10T05:49:14Z
```

# Clarify 'uv python upgrade' message when no versions exist

---

_Pull request opened by @sinsniwal on 2025-11-21 22:25_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves : #16783

That message is only visible if uv found no managed Python installations. If you had Python installed (even if it was fully up-to-date), requests would not be empty, and the code would skip this block and move on to the resolution phase.

The output message "There are no installed versions to upgrade" was ambiguous. It was unclear whether `uv` could not find any installed versions or if the existing versions were simply already up-to-date as mentioned in issue.

So updated the message to 
```"No Python installations found; run `uv python install` to install a Python version.```
 to explicitly indicate when no `uv`-managed Python installations exist.

## Test Plan

Updated the existing test snapshot in `python_upgrade_without_version` to match the new output string. Verified that tests pass locally with `cargo test`.

---

_@zanieb reviewed on 2025-11-21 22:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:625 on 2025-11-21 22:28_

Is this case entirely redundant with the one above?

---

_@zanieb reviewed on 2025-11-21 22:30_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:314 on 2025-11-21 22:30_

I wonder if we should use our `hint: ...` styling for the second part of the message?

We should style the command with color like we do for other commands in output.

nit: I'd probably say "use" instead of "run".

---

_@sinsniwal reviewed on 2025-11-21 22:51_

---

_Review comment by @sinsniwal on `crates/uv/src/commands/python/install.rs`:625 on 2025-11-21 22:51_

Yes, makes sense. I've applied those changes in the new commit.

---

_Review comment by @sinsniwal on `crates/uv/src/commands/python/install.rs`:314 on 2025-11-21 22:52_

sure , i have added those changes in the new commit, thanks.

---

_@sinsniwal reviewed on 2025-11-21 22:52_

---

_@zanieb reviewed on 2025-11-21 23:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:314 on 2025-11-21 23:06_

The hint formatting I was referring to is like https://github.com/astral-sh/uv/blob/60a811e715d1d1919afd73b0e4dec4c4be8aa3f9/crates/uv-python/src/interpreter.rs#L863-L869

---

_Label `error messages` added by @zanieb on 2025-11-21 23:09_

---

_@sinsniwal reviewed on 2025-11-22 00:02_

---

_Review comment by @sinsniwal on `crates/uv/src/commands/python/install.rs`:314 on 2025-11-22 00:02_

Made the changes

---

_Comment by @sinsniwal on 2025-11-24 15:47_

Hi @zanieb , just following up, have applied the suggestions from the last review.

---

_Comment by @zanieb on 2025-11-24 22:14_

I can't seem to push to your branch, here are the edits I'd suggest:

```diff
commit 72ed47cd2c4f2718628d2102521eda7805a6a534 (HEAD -> sinsniwal/main)
Author: Zanie Blue <contact@zanie.dev>
Date:   Mon Nov 24 16:13:28 2025 -0600

    Review

diff --git a/crates/uv/src/commands/python/install.rs b/crates/uv/src/commands/python/install.rs
index 2b1ee3066..db52b90f4 100644
--- a/crates/uv/src/commands/python/install.rs
+++ b/crates/uv/src/commands/python/install.rs
@@ -249,17 +249,6 @@ pub(crate) async fn install(
         ) {
             is_unspecified_upgrade = true;
 
-            if existing_installations.is_empty() {
-                writeln!(
-                    printer.stderr(),
-                    "No Python installations found.\n{}{} Use `{}` to install a Python version.",
-                    "hint".bold().cyan(),
-                    ":".bold(),
-                    "uv python install".green()
-                )?;
-                return Ok(ExitStatus::Success);
-            }
-
             // On upgrade, derive requests for all of the existing installations
             let mut minor_version_requests = IndexSet::<InstallRequest>::default();
             for installation in &existing_installations {
@@ -315,13 +304,22 @@ pub(crate) async fn install(
 
     if requests.is_empty() {
         match upgrade {
+            PythonUpgrade::Enabled(PythonUpgradeSource::Upgrade) => {
+                writeln!(
+                    printer.stderr(),
+                    "No managed Python installations found\n\n{}{} Use `{}` to install a new Python version",
+                    "hint".bold().cyan(),
+                    ":".bold(),
+                    "uv python install".green()
+                )?;
+            }
             PythonUpgrade::Enabled(PythonUpgradeSource::Install) => {
                 writeln!(
                     printer.stderr(),
                     "No Python versions specified for upgrade; did you mean `uv python upgrade`?"
                 )?;
             }
-            PythonUpgrade::Disabled | PythonUpgrade::Enabled(PythonUpgradeSource::Upgrade) => {}
+            PythonUpgrade::Disabled => {}
         }
         return Ok(ExitStatus::Success);
     }
diff --git a/crates/uv/tests/it/python_upgrade.rs b/crates/uv/tests/it/python_upgrade.rs
index ac0461125..27ea140d4 100644
--- a/crates/uv/tests/it/python_upgrade.rs
+++ b/crates/uv/tests/it/python_upgrade.rs
@@ -106,8 +106,9 @@ fn python_upgrade_without_version() {
     ----- stdout -----
 
     ----- stderr -----
-    No Python installations found.
-    hint: Use `uv python install` to install a Python version.
+    No managed Python installations found
+
+    hint: Use `uv python install` to install a new Python version
     ");
 
     // Install earlier patch versions for different minor versions
```

---

_Comment by @sinsniwal on 2025-11-24 23:33_

I have made the changes. Thank you for the help.

---

_Comment by @sinsniwal on 2025-11-28 02:26_

@zanieb lgtm, can you please review 

---
