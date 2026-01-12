```yaml
number: 12943
title: "fix(ui): update version formatting to use cyan color"
type: pull_request
state: merged
author: kdheepak
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: kd/remove-white
created_at: 2025-04-17T13:07:02Z
updated_at: 2025-04-17T21:23:32Z
url: https://github.com/astral-sh/uv/pull/12943
synced_at: 2026-01-12T16:10:27Z
```

# fix(ui): update version formatting to use cyan color

---

_@kdheepak_

## Summary

This PR simplifies the version formatting by replacing `.white()` with `.cyan()` styling for consistency.

Resolves #12940 

## Test Plan

I manually recreated the code and tested it with this patch:

```diff
diff --git i/crates/uv/src/lib.rs w/crates/uv/src/lib.rs
index b9c01b002..cf051351f 100644
--- i/crates/uv/src/lib.rs
+++ w/crates/uv/src/lib.rs
@@ -1019,6 +1019,20 @@ async fn run(mut cli: Cli) -> Result<ExitStatus> {
         }) => commands::self_update(target_version, token, printer).await,
         #[cfg(not(feature = "self-update"))]
         Commands::Self_(_) => {
+            eprintln!("{}: {}", "error".cyan().bold(), "fake error message");
+
+            let version_information = format!(
+                "from {} to {}",
+                "v0.1.1".bold().cyan(),
+                "v0.1.2".bold().cyan(),
+            );
+            eprintln!(
+                "{}{} Upgraded uv {}! {}",
+                "success".green().bold(),
+                ":".bold(),
+                version_information,
+                format!("https://github.com/astral-sh/uv/releases/tag/{}", "v0.1.2").cyan()
+            );
             anyhow::bail!(
                 "uv was installed through an external package manager, and self-update \
                 is not available. Please use your package manager to update uv."
```

In a light terminal, this is what it looks like:

<img width="750" alt="image" src="https://github.com/user-attachments/assets/dc0d283c-e845-41fb-9821-80b0a3f1c4fe" />


---

_Comment by @charliermarsh on 2025-04-17 13:09_

Let's use cyan please :)

---

_Label `bug` added by @charliermarsh on 2025-04-17 13:09_

---

_Label `cli` added by @charliermarsh on 2025-04-17 13:09_

---

_Renamed from "fix(ui): remove `.white()` styling in version output" to "fix(ui): update version formatting to use cyan color" by @kdheepak on 2025-04-17 14:55_

---

_Comment by @kdheepak on 2025-04-17 15:10_

I made the change, force pushed and updated the PR title and description.

---

_@charliermarsh approved on 2025-04-17 16:58_

Thanks!

---

_Merged by @charliermarsh on 2025-04-17 16:58_

---

_Closed by @charliermarsh on 2025-04-17 16:58_

---

_Branch deleted on 2025-04-17 21:23_

---
