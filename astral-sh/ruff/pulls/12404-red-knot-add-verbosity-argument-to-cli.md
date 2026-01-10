```yaml
number: 12404
title: "[red-knot] Add verbosity argument to CLI"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-cli-verbosity
created_at: 2024-07-19T10:35:45Z
updated_at: 2024-07-19T11:48:10Z
url: https://github.com/astral-sh/ruff/pull/12404
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Add verbosity argument to CLI

---

_Pull request opened by @MichaReiser on 2024-07-19 10:35_

## Summary

This PR adds a new `--verbose` and `-v`, `-vv` and `-vvv` options and uses the level to configure the tracing verbosity.

## Test Plan

I ran red knot with the different settings and verified that traces appear according to that level.


---

_Label `red-knot` added by @MichaReiser on 2024-07-19 10:37_

---

_Marked ready for review by @MichaReiser on 2024-07-19 10:37_

---

_Review requested from @carljm by @MichaReiser on 2024-07-19 10:37_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-19 10:37_

---

_@MichaReiser reviewed on 2024-07-19 10:42_

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:138 on 2024-07-19 10:42_

Sorry for the renames but clippy became very angry at me and told me that the old name was a bad name.

---

_Comment by @AlexWaygood on 2024-07-19 10:50_

It looks a _litle_ odd right now that `--verbose` is the only CLI option that doesn't have a description when you run `--help`:

![image](https://github.com/user-attachments/assets/93c21fc8-73c1-471e-b1ca-0a0c3145eee2)


---

_@AlexWaygood reviewed on 2024-07-19 10:57_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cli/verbosity.rs`:33 on 2024-07-19 10:57_

I feel like this could be simplified if you added a `VerbosityLevel::Warn` variant and just returned `VerbosityLevel` from this method, rather than `Option<VerbosityLevel>`. Something like this?

```diff
diff --git a/crates/red_knot/src/cli/verbosity.rs b/crates/red_knot/src/cli/verbosity.rs
index 30bc2a4a5..470bfa49c 100644
--- a/crates/red_knot/src/cli/verbosity.rs
+++ b/crates/red_knot/src/cli/verbosity.rs
@@ -1,5 +1,6 @@
 #[derive(Debug, Copy, Clone, Eq, PartialEq, Ord, PartialOrd)]
 pub(crate) enum VerbosityLevel {
+    Warn,
     Info,
     Debug,
     Trace,
@@ -19,12 +20,12 @@ pub(crate) struct Verbosity {
 }
 
 impl Verbosity {
-    pub(crate) fn level(&self) -> Option<VerbosityLevel> {
+    pub(crate) fn level(&self) -> VerbosityLevel {
         match self.verbose {
-            0 => None,
-            1 => Some(VerbosityLevel::Info),
-            2 => Some(VerbosityLevel::Debug),
-            _ => Some(VerbosityLevel::Trace),
+            0 => VerbosityLevel::Warn,
+            1 => VerbosityLevel::Info,
+            2 => VerbosityLevel::Debug,
+            _ => VerbosityLevel::Trace,
         }
     }
 }
diff --git a/crates/red_knot/src/main.rs b/crates/red_knot/src/main.rs
index 35bfe2380..f39a23fd2 100644
--- a/crates/red_knot/src/main.rs
+++ b/crates/red_knot/src/main.rs
@@ -75,7 +75,7 @@ pub fn main() -> anyhow::Result<()> {
     } = Args::parse_from(std::env::args().collect::<Vec<_>>());
 
     let verbosity = verbosity.level();
-    countme::enable(verbosity == Some(VerbosityLevel::Trace));
+    countme::enable(verbosity >= VerbosityLevel::Trace);
     setup_tracing(verbosity);
 
     let cwd = if let Some(cwd) = current_directory {
@@ -134,13 +134,13 @@ pub fn main() -> anyhow::Result<()> {
 }
 
 struct MainLoop {
-    verbosity: Option<VerbosityLevel>,
+    verbosity: VerbosityLevel,
     orchestrator: crossbeam_channel::Sender<OrchestratorMessage>,
     receiver: crossbeam_channel::Receiver<MainLoopMessage>,
 }
 
 impl MainLoop {
-    fn new(verbosity: Option<VerbosityLevel>) -> (Self, MainLoopCancellationToken) {
+    fn new(verbosity: VerbosityLevel) -> (Self, MainLoopCancellationToken) {
         let (orchestrator_sender, orchestrator_receiver) = crossbeam_channel::bounded(1);
         let (main_loop_sender, main_loop_receiver) = crossbeam_channel::bounded(1);
 
@@ -203,12 +203,12 @@ impl MainLoop {
                 }
                 MainLoopMessage::CheckCompleted(diagnostics) => {
                     eprintln!("{}", diagnostics.join("\n"));
-                    if self.verbosity == Some(VerbosityLevel::Trace) {
+                    if self.verbosity >= VerbosityLevel::Trace {
                         eprintln!("{}", countme::get_all());
                     }
                 }
                 MainLoopMessage::Exit => {
-                    if self.verbosity == Some(VerbosityLevel::Trace) {
+                    if self.verbosity >= VerbosityLevel::Trace {
                         eprintln!("{}", countme::get_all());
                     }
                     return;
@@ -361,12 +361,12 @@ enum OrchestratorMessage {
     FileChanges(Vec<FileWatcherChange>),
 }
 
-fn setup_tracing(verbosity: Option<VerbosityLevel>) {
+fn setup_tracing(verbosity: VerbosityLevel) {
     let trace_level = match verbosity {
-        None => Level::WARN,
-        Some(VerbosityLevel::Info) => Level::INFO,
-        Some(VerbosityLevel::Debug) => Level::DEBUG,
-        Some(VerbosityLevel::Trace) => Level::TRACE,
+        VerbosityLevel::Warn => Level::WARN,
+        VerbosityLevel::Info => Level::INFO,
+        VerbosityLevel::Debug => Level::DEBUG,
+        VerbosityLevel::Trace => Level::TRACE,
     };
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/cli/verbosity.rs`:33 on 2024-07-19 11:06_

Yeah, I started with this but I disliked the name warn for the default. What the option indicates is that the user didn't specify any verbosity. I considered using `DEFAULT`, but then went with `Option`. 

We probably need to revisit this once we add other means of logging messages to the user. 

---

_@MichaReiser reviewed on 2024-07-19 11:06_

---

_Comment by @github-actions[bot] on 2024-07-19 11:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-07-19 11:08_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cli/verbosity.rs`:33 on 2024-07-19 11:08_

Maybe `Unspecified`? Right now it feels unclear to me, as a reader of the code, why it returns `Option`, so I'd at least add a comment explaining the decision, I think :-)

---

_@AlexWaygood reviewed on 2024-07-19 11:09_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/cli/verbosity.rs`:15 on 2024-07-19 11:09_

```suggestion
        help = "Use more verbose output (can be specified multiple times)",
```

---

_Merged by @MichaReiser on 2024-07-19 11:38_

---

_Closed by @MichaReiser on 2024-07-19 11:38_

---

_Branch deleted on 2024-07-19 11:38_

---
