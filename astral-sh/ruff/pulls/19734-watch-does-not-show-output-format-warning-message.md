```yaml
number: 19734
title: watch does not show output format warning message
type: pull_request
state: closed
author: mikeleppane
labels:
  - cli
assignees: []
base: main
head: fix(19552)/watch-does-not-show-output-format-warning-message
created_at: 2025-08-04T08:56:17Z
updated_at: 2025-11-22T14:23:45Z
url: https://github.com/astral-sh/ruff/pull/19734
synced_at: 2026-01-10T16:48:01Z
```

# watch does not show output format warning message

---

_Pull request opened by @mikeleppane on 2025-08-04 08:56_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Added and fixed how output formatting works in watch mode

### Changes Made


* Created `format_diagnostics()` method to support different write modes `WriteMode::Once` vs `WriteMode::Continuous`
* Added proper handling of `OutputFormat::Full` and `OutputFormat::Concise` in `watch` mode through the new `WriteMode::Continuous` branch
* Ensured that full diagnostic formatting (including source code snippets and detailed error context) is preserved when using --watch with --output-format full


* Introduced `WriteMode` enum to distinguish between single-run and continuous watch mode operations
* Updated `TextEmitter` configuration in continuous mode to respect the `OutputFormat::Full` setting via .with_show_source(self.format == OutputFormat::Full)
* Maintained backward compatibility by preserving existing behavior for non-watch mode operations

Note: with these changes, selecting `--preview` mode does not have any effect on output formatting. `full` mode is still the default mode. 

Related issues: [#19552](https://github.com/astral-sh/ruff/issues/19552), [#19550](https://github.com/astral-sh/ruff/issues/19550)

## Test Plan

* Added three integration tests to verify that watch mode properly outputs full diagnostic formatting
* Tests cover scenarios with both errors present and no errors found
* Ensured cross-platform compatibility for file path handling in test snapshots


---

_Renamed from "Fix(19552)/watch does not show output format warning message" to "watch does not show output format warning message" by @MichaReiser on 2025-08-04 08:57_

---

_Comment by @github-actions[bot] on 2025-08-04 09:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:08_

---

_Review comment by @ntBre on `crates/ruff/src/lib.rs`:359 on 2025-08-08 19:19_

I don't think we need to log the format here. The user just typed `--output-format ...` in their CLI invocation. The warning was only because the format wasn't being used before.

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:370 on 2025-08-08 19:21_

Why do we need an empty line here? Was that coming from somewhere else before?

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:2436 on 2025-08-08 19:25_

Is this necessary? It seems like running the command like this should mean Ruff knows it's not writing to a tty and should suppress colors automatically.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:2430 on 2025-08-08 19:25_

```suggestion
```

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:2439 on 2025-08-08 19:37_

These tests are cool, but I'm not sure they're worth the effort. They require a pretty substantial amount of setup code, and the timeouts make me feel like they could be flaky too. I think I'd be okay with a manual test in this case. The output formats are well-tested otherwise, so we only have to check that we've wired up the `--watch` flag with them correctly. I'd be interested in what you and @MichaReiser think about that.

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:467 on 2025-08-08 19:40_

This should also be


```suggestion
                        .with_show_source(preview)
```

if we want to maintain the current behavior, which I think we should do until a minor release.

---

_Review comment by @ntBre on `crates/ruff/src/printer.rs`:399 on 2025-08-08 19:56_

It would be a bit more intuitive to me if we could call `write_once` here. It seems like we're pretty close to being able to do that, likely with one more flag controlling the summary text. Would that be doable, or do we need this third method? 

I think the `full` preview behavior might actually be one of the trickiest parts of that.

---

_@ntBre reviewed on 2025-08-08 19:59_

Thank you! This looks good to me overall. I just had a few minor suggestions and one testing question that we may want to get Micha's opinion on too.

---

_Label `cli` added by @ntBre on 2025-08-08 19:59_

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:445 on 2025-08-11 08:06_

I wonder if `WriteMode` is necessary or if it's sufficient to only check `self.flags` instead. 

E.g. `.with_show_source(self.format == OutputFormat::Full)` could be handled at the call site instead (where it sets the output format to concise unless we're in preview).
or `.with_show_fix_diff(self.flags.intersects(Flags::SHOW_FIX_DIFF))`: I think we can leave it up to the caller whether it sets this flag or not. 



---

_Review comment by @MichaReiser on `crates/ruff/tests/integration_test.rs`:2439 on 2025-08-11 08:11_

I agree with Brent here. The main value that I see in file watching tests is if the "react to changes" works as expected which these tests aren't testing (see `file_watching.rs` in ty). Having tests for the watch CLI output is great but I'd rather remove as many differences between the two outputs as possible so that file watching CLI tests become unnecessary (that's whayt we do in ty). I think making the two commands behave more similarly also benefits users because there are fewer surprises between the two.

---

_@MichaReiser reviewed on 2025-08-18 07:33_

---

_Comment by @MichaReiser on 2025-11-21 08:05_

@ntBre this seems like another PR that's very close to landing and only requires minimal cleanup. Would it makes ense for you to do them yourself so that we can get this PR landed?

---

_Comment by @ntBre on 2025-11-21 18:30_

I was kind of tempted not to land this after https://github.com/astral-sh/ruff/pull/21097, but I guess it still makes sense to warn until it's out of preview. I'll take a look!

---

_Comment by @ntBre on 2025-11-21 19:10_

Hmm, yeah I'm inclined to close this or just repurpose it to remove the warning code. The current state of the branch doesn't help with showing the warning (it actually deletes the warning as far as I can tell?), and  the best alternative for that I can see is something like:

```diff
diff --git a/crates/ruff/src/lib.rs b/crates/ruff/src/lib.rs
index 3ea0d94fad..995dec44b2 100644
--- a/crates/ruff/src/lib.rs
+++ b/crates/ruff/src/lib.rs
@@ -358,13 +358,6 @@ pub fn check(args: CheckCommand, global_options: GlobalConfigArgs) -> Result<Exi
     let preview = pyproject_config.settings.linter.preview.is_enabled();
 
     if cli.watch {
-        if output_format != OutputFormat::default() {
-            warn_user!(
-                "`--output-format {}` is always used in watch mode.",
-                OutputFormat::default()
-            );
-        }
-
         // Configure the file watcher.
         let (tx, rx) = channel();
         let mut watcher = recommended_watcher(tx)?;
@@ -377,6 +370,17 @@ pub fn check(args: CheckCommand, global_options: GlobalConfigArgs) -> Result<Exi
 
         // Perform an initial run instantly.
         Printer::clear_screen()?;
+
+        // This doesn't work exactly right because we won't flag `--output-format=full` (the
+        // default) without preview mode, but we can't really differentiate `--output-format=full`
+        // from the flag not being passed at this point.
+        if output_format != OutputFormat::default() && !preview {
+            printer.write_to_user(format_args!(
+                "`--output-format {}` is always used in watch mode unless preview is enabled.\n",
+                OutputFormat::Concise
+            ));
+        }
+
         printer.write_to_user("Starting linter in watch mode...\n");
 
         let diagnostics = commands::check::check(
diff --git a/crates/ruff/src/printer.rs b/crates/ruff/src/printer.rs
index 1de440ac6e..f5329f70ad 100644
--- a/crates/ruff/src/printer.rs
+++ b/crates/ruff/src/printer.rs
@@ -62,7 +62,7 @@ impl Printer {
         }
     }
 
-    pub(crate) fn write_to_user(&self, message: &str) {
+    pub(crate) fn write_to_user(&self, message: impl std::fmt::Display) {
         if self.log_level >= LogLevel::Default {
             notify_user!("{}", message);
         }
```

but that won't emit a warning for `--output-format=full` since it's the `OutputFormat::default()`. And I expect that to be the most common alternative output format someone would try.

I don't think #21097 has been too controversial, so we should just stabilize it in the next minor and resolve this too.

---

_Comment by @MichaReiser on 2025-11-22 10:51_

Sounds good. 

---

_Comment by @MichaReiser on 2025-11-22 14:23_

Thanks for your work @mikeleppane In preview, Ruff now respects the `--output-format` message which will fix https://github.com/astral-sh/ruff/issues/19552 once stabilizied

---

_Closed by @MichaReiser on 2025-11-22 14:23_

---
