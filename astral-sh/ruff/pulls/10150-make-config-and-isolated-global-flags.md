```yaml
number: 10150
title: "Make `--config` and `--isolated` global flags"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - cli
assignees: []
merged: true
base: main
head: config-global-option
created_at: 2024-02-28T13:07:26Z
updated_at: 2024-07-01T10:29:58Z
url: https://github.com/astral-sh/ruff/pull/10150
synced_at: 2026-01-10T21:55:59Z
```

# Make `--config` and `--isolated` global flags

---

_Pull request opened by @AlexWaygood on 2024-02-28 13:07_

## Summary

Fixes #9892.

This PR makes `--config` and `--isolated` global flags that are accepted by all ruff subcommands: ruff will no longer error out if a redundant `--config` argument is passed to a command such as `ruff version` or `ruff rule`. The flag has no effect for subcommands that do not need it (but it is still validated).

## Test Plan

There were no tests for `ruff version`, so I added them as part of this PR, and used the new tests to assert that the `--config` flag is now accepted by `ruff version`. I also manually tested to check things were working as expected.

---

_@AlexWaygood reviewed on 2024-02-28 13:09_

---

_Review comment by @AlexWaygood on `crates/ruff/src/lib.rs`:32 on 2024-02-28 13:09_

I made this change as I couldn't otherwise figure out how to access functions from the `version` module in `crates/ruff/tests/version.rs`. But this might be a Rust newbie thing 🙃. Lmk if there's a better solution here!

---

_Review comment by @MichaReiser on `crates/ruff/src/version.rs`:74 on 2024-02-28 13:11_

I would duplicate this information in the test instead of making the module public. Especially beause the `version_string` function isn't used in the implementation code. 

---

_@MichaReiser reviewed on 2024-02-28 13:11_

---

_Review comment by @MichaReiser on `crates/ruff/src/version.rs`:75 on 2024-02-28 13:13_

I would prefer using `filters` in the test over making this function and the entire module public. 

---

_Review comment by @AlexWaygood on `crates/ruff/src/version.rs`:74 on 2024-02-28 13:17_

I wondered about doing this (diff is relative to this PR branch):

```diff
diff --git a/crates/ruff/src/version.rs b/crates/ruff/src/version.rs
index 6de4b2534..194dd62c7 100644
--- a/crates/ruff/src/version.rs
+++ b/crates/ruff/src/version.rs
@@ -14,7 +14,7 @@ pub(crate) struct CommitInfo {
 
 /// Ruff's version.
 #[derive(Serialize)]
-pub(crate) struct VersionInfo {
+pub struct VersionInfo {
     /// Ruff's version, such as "0.5.1"
     version: String,
     /// Information about the git commit we may have been built from.
@@ -40,7 +40,7 @@ impl fmt::Display for VersionInfo {
 }
 
 /// Returns information about Ruff's version.
-pub(crate) fn version() -> VersionInfo {
+pub fn version() -> VersionInfo {
     // Environment variables are only read at compile-time
     macro_rules! option_env_str {
         ($name:expr) => {
@@ -68,11 +68,6 @@ pub(crate) fn version() -> VersionInfo {
     }
 }
 
-/// Used for testing the --version command
-pub fn version_string() -> String {
-    format!("{}", version())
-}
-
 #[cfg(test)]
 mod tests {
     use insta::{assert_display_snapshot, assert_json_snapshot};
diff --git a/crates/ruff/tests/version.rs b/crates/ruff/tests/version.rs
index 69e8ea1ca..360747722 100644
--- a/crates/ruff/tests/version.rs
+++ b/crates/ruff/tests/version.rs
@@ -7,10 +7,12 @@ use insta_cmd::{assert_cmd_snapshot, get_cargo_bin};
 use regex::escape;
 use tempfile::TempDir;
 
-use ruff::version::version_string;
-
 const BIN_NAME: &str = "ruff";
 
+fn version_string() -> String {
+    format!("{}", ruff::version::version())
+}
+
 #[test]
```

But that actually exposes more of the module publicly rather than less (which is why I went with what I have currently)

---

_@AlexWaygood reviewed on 2024-02-28 13:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:49 on 2024-02-28 13:20_

What does `global` do? Is it necessary considering that it is defined at the top level arguments?

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:70 on 2024-02-28 13:21_

I don't have a good suggestion. `ArgGroup` could be what we want but I'm not sure.

What I'm thinking is that it would be nice to group the global settings (potentially even with a different header that these are global options). The benefit of grouping the arguments in a struct is that we could pass them around as a single argument instead of passing each value separately (doesn't scale well)

---

_@MichaReiser approved on 2024-02-28 13:22_

---

_Review comment by @MichaReiser on `docs/configuration.md`:595 on 2024-02-28 13:22_

It feels a bit strange that `--isolated` is in `Miscellaneous` and `--config` is not. I think I would move `--config`

---

_@MichaReiser reviewed on 2024-02-28 13:22_

---

_@MichaReiser reviewed on 2024-02-28 13:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/version.rs`:74 on 2024-02-28 13:24_

Sorry yeah, see my other comment (I deleted it but didn't realise that I submitted it right away). I recommend using filters here.

---

_@AlexWaygood reviewed on 2024-02-28 13:24_

---

_Review comment by @AlexWaygood on `crates/ruff/src/version.rs`:75 on 2024-02-28 13:24_

Good point, I can probably just use a regex in the filter rather than `re.escap`ing the exact version in the filter... thanks :)

---

_Comment by @github-actions[bot] on 2024-02-28 13:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood reviewed on 2024-02-28 13:29_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:49 on 2024-02-28 13:29_

It's necessary, yes -- without `global = true`, `--config` would only be accepted as a flag for the _top-level_ command (just `ruff`). With `global = true`, it's accepted for all subcommands as well as the top-level command: `ruff check`, `ruff format`, `ruff rule`, etc.

---

_@AlexWaygood reviewed on 2024-02-28 13:44_

---

_Review comment by @AlexWaygood on `crates/ruff/tests/version.rs`:11 on 2024-02-28 13:44_

Possibly this regex is a little overboard 😆 I'd be okay with going with something a little more vague if you prefer, e.g.

```suggestion
    r"\d+\.\d+\.\d+.*",
```

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:49 on 2024-02-28 13:55_

That makes sense. Thanks for explaining

---

_@MichaReiser reviewed on 2024-02-28 13:55_

---

_Label `cli` added by @MichaReiser on 2024-02-28 13:55_

---

_@MichaReiser reviewed on 2024-02-28 16:04_

---

_Review comment by @MichaReiser on `crates/ruff/tests/version.rs`:11 on 2024-02-28 16:04_

I don't have a strong preference. I prefer simple regex because I can't read regex expressions but 🤷 

---

_@AlexWaygood reviewed on 2024-02-28 16:45_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:70 on 2024-02-28 16:45_

I had a go at creating a struct to group them altogether in https://github.com/astral-sh/ruff/pull/10150/commits/664beb42ddd54d1976aafad41fe9b71a6c3d03fb, but I'm not sure... it's hard in particular to think of a good name for the struct. The word "config/configuration" is getting quite overloaded...

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-02-28 16:45_

---

_Review comment by @MichaReiser on `docs/configuration.md`:543 on 2024-02-29 10:08_

```suggestion
Global options:
```

To align with `Options` (it's not Configuration options)

---

_@MichaReiser approved on 2024-02-29 10:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:23_

Uh, `clap` supports `flatten`. Can we use 

```
    #[clap(flatten)]
    global: GlobalConfigArgs
```

The `GlobalConfigArgs` then also fits better into the naming convention where clap parses into `*Args` 

---

_@MichaReiser reviewed on 2024-02-29 12:23_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:569 on 2024-02-29 12:27_

`pub(crate)` should be sufficient

---

_@MichaReiser reviewed on 2024-02-29 12:28_

Improvements that we could make that are unrelated to your change but could improve readability (as separate PRs):

* Rename `HelpFormat` to `OutputFormat`. 
* Move `ConfigArguments` next to `FormatArguments` and `CheckArguments`.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:32_

What's the relation between `log_level` and `Args::log_level_args`. How do we ensure that the two are in sync?

---

_@MichaReiser reviewed on 2024-02-29 12:32_

---

_@AlexWaygood reviewed on 2024-02-29 12:46_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:46_

`log_level_args` is an instance of the `LogLevelArgs` struct that contains information about the various logging-related flags the user may or may not have passed on the CLI.

`log_level` is a variant of the `LogLevel` enum: the flags have now been parsed internally so that we now know what logging level to apply henceforth.

The `LogLevelArgs` struct is parsed into a `LogLevel` variant in `Args::partition()`

---

_@AlexWaygood reviewed on 2024-02-29 12:50_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:50_

I'll have a play with this.

---

_@AlexWaygood reviewed on 2024-02-29 12:51_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:51_

it's quite nice to eagerly parse `LogLevelArgs` into more structured data, as I'm doing now, but I think that might be trickier if we use `flatten` like you suggest in https://github.com/astral-sh/ruff/pull/10150#discussion_r1507492780

---

_@MichaReiser reviewed on 2024-02-29 12:55_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:55_

Oh I see, `Args`  has a `partition` too and `GlobalConfigArgs` (as of today) is not really a `Args` but more an `Arguments` (it's not populated by clap).

I think we can make `log_level` a function on `GlobalConfigArgs` and inline `LogLevelArgs` into `GlobalConfigArgs`

---

_@AlexWaygood reviewed on 2024-02-29 12:57_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:35 on 2024-02-29 12:57_

Ahh that sounds like a nice solution. I'll try it after lunch.

---

_Comment by @AlexWaygood on 2024-02-29 14:58_

Alright, ready for another look, I think 😅 the implementation now uses `clap(flatten)`, as you suggested!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-02-29 14:58_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:67 on 2024-02-29 18:31_

Nit: Can we derive default instead? Or use setters for the optional values? Or do we have call sites that need to set all values?

---

_@MichaReiser reviewed on 2024-03-04 10:03_

---

_@MichaReiser approved on 2024-03-04 10:05_

---

_Merged by @AlexWaygood on 2024-03-04 11:19_

---

_Closed by @AlexWaygood on 2024-03-04 11:19_

---

_Branch deleted on 2024-07-01 10:29_

---
