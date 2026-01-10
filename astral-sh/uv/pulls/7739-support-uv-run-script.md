```yaml
number: 7739
title: "Support `uv run --script`"
type: pull_request
state: merged
author: tfsingh
labels:
  - cli
assignees: []
merged: true
base: main
head: tfsingh/script-run
created_at: 2024-09-27T13:47:39Z
updated_at: 2024-10-11T09:53:31Z
url: https://github.com/astral-sh/uv/pull/7739
synced_at: 2026-01-10T12:53:54Z
```

# Support `uv run --script`

---

_Pull request opened by @tfsingh on 2024-09-27 13:47_

This PR adds support for executing a script with ```uv run```, even when the script does not have a ```.py``` extension. Addresses #7396.

---

_Review comment by @tfsingh on `crates/uv-cli/src/lib.rs`:2555 on 2024-09-27 13:50_

Not sure what the best approach to arg parsing is here — we could explicitly enumerate conflicts (such as ```--extra``` and ```--all-extras```), but we don't even warn for these as is when running a script (implicitly) so not sure if it's worth doing. Happy to either way!

---

_Review comment by @tfsingh on `crates/uv/src/lib.rs`:140 on 2024-09-27 13:51_

This was lifted from RunCommand::try_from, I thought it best to not meddle with that API by adding a ```script``` parameter 

---

_@tfsingh reviewed on 2024-09-27 13:51_

---

_Marked ready for review by @tfsingh on 2024-09-27 18:42_

---

_Comment by @zanieb on 2024-10-01 19:45_

I think I'd explore something like this instead?

```diff
diff --git a/crates/uv/src/commands/project/run.rs b/crates/uv/src/commands/project/run.rs
index 2dc660e34..a375d126e 100644
--- a/crates/uv/src/commands/project/run.rs
+++ b/crates/uv/src/commands/project/run.rs
@@ -862,6 +862,17 @@ impl RunCommand {
         }
     }
 
+    pub(crate) fn force_python_script(self) -> anyhow::Result<Self> {
+        match self {
+            Self::Empty => Ok(self),
+            Self::PythonScript(..) | Self::PythonGuiScript(..) => Ok(self),
+            Self::PythonPackage(..) => {
+                bail!("Target is a directory and cannot be executed as a script")
+            }
+            _ => todo!(),
+        }
+    }
+
     /// Convert a [`RunCommand`] into a [`Command`].
     fn as_command(&self, interpreter: &Interpreter) -> Command {
         match self {
diff --git a/crates/uv/src/lib.rs b/crates/uv/src/lib.rs
index 68efb85b1..b0de789fc 100644
--- a/crates/uv/src/lib.rs
+++ b/crates/uv/src/lib.rs
@@ -135,17 +135,12 @@ async fn run(cli: Cli) -> Result<ExitStatus> {
             command, script, ..
         }) = &**command
         {
-            if *script {
-                let (target, args) = command.split();
-                if let Some(target) = target {
-                    let path = PathBuf::from(&target);
-                    Some(RunCommand::PythonScript(path, args.to_vec()))
-                } else {
-                    anyhow::bail!("Script path must be supplied when using `uv run --script`");
-                }
+            let command = RunCommand::try_from(command)?;
+            Some(if *script {
+                command.force_python_script()?
             } else {
-                Some(RunCommand::try_from(command)?)
-            }
+                command
+            })
         } else {
             None
         }
```

---

_Comment by @zanieb on 2024-10-01 19:57_

Looks like `RunComand::from_args` was added for `--module` support, which is also a pretty reasonable approach and simplifies your problem here.

---

_@zanieb reviewed on 2024-10-01 23:27_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:2170 on 2024-10-01 23:27_

Can you add test cases for `--script` with a file that does not exist and a directory?

---

_@tfsingh reviewed on 2024-10-01 23:40_

---

_Review comment by @tfsingh on `crates/uv/tests/run.rs`:2170 on 2024-10-01 23:40_

Done!

---

_Review requested from @charliermarsh by @zanieb on 2024-10-01 23:48_

---

_Label `cli` added by @zanieb on 2024-10-02 14:50_

---

_@zanieb approved on 2024-10-02 14:51_

Thanks!

---

_Merged by @zanieb on 2024-10-02 14:51_

---

_Closed by @zanieb on 2024-10-02 14:51_

---

_Renamed from "Support uv run --script" to "Support `uv run --script`" by @charliermarsh on 2024-10-11 09:53_

---
