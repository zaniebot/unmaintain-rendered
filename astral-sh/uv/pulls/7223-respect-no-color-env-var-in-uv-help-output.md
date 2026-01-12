```yaml
number: 7223
title: "Respect `NO_COLOR` env var in `uv help` output"
type: pull_request
state: open
author: blueraft
labels: []
assignees: []
base: main
head: no-colors
created_at: 2024-09-09T16:33:09Z
updated_at: 2025-04-28T12:59:19Z
url: https://github.com/astral-sh/uv/pull/7223
synced_at: 2026-01-12T16:07:44Z
```

# Respect `NO_COLOR` env var in `uv help` output

---

_@blueraft_

## Summary

Resolves #7219 

## Test Plan

<img width="1194" alt="Screenshot 2024-09-09 at 18 32 37" src="https://github.com/user-attachments/assets/23fd1506-5dab-4897-8b87-2cc65fe56baf">

<img width="813" alt="Screenshot 2024-09-10 at 09 40 56" src="https://github.com/user-attachments/assets/0610c0a5-e5d9-49db-8544-5165dba981b5">


---

_Comment by @zanieb on 2024-09-09 20:54_

Yeah it seems hard. Does Clap stop parsing options when it encounters `--help`? Do we need to invoke the short-help callback ourselves?

---

_Comment by @eth3lbert on 2024-09-10 00:12_

We have already disabled the default help flag and replaced it with our own but with `action = clap::ArgAction::HelpShort`. 

It's uncertain if it's worthwhile, but I believe you could achieve this by first mutating the `help` argument and changing its action to `clap::ArgAction::SetTrue`, then attempting to get matches with errors disabled.

Something like the following should work:
```diff
diff --git a/crates/uv/src/lib.rs b/crates/uv/src/lib.rs
index df4780c0..d3bbb56e 100644
--- a/crates/uv/src/lib.rs
+++ b/crates/uv/src/lib.rs
@@ -1359,9 +1359,22 @@ where
 {
     // `std::env::args` is not `Send` so we parse before passing to our runtime
     // https://github.com/rust-lang/rust/pull/48005
+    let args = args.into_iter().collect::<Vec<_>>();
+    let matches = Cli::command()
+        .mut_arg("help", |arg| arg.action(clap::ArgAction::SetTrue))
+        .ignore_errors(true)
+        .try_get_matches_from(args.clone());
+    let no_color = matches.is_ok_and(|arg| {
+        matches!(
+            arg.try_get_one::<uv_cli::ColorChoice>("color"),
+            Ok(Some(uv_cli::ColorChoice::Never))
+        )
+    });
     let cli = match Cli::try_parse_from(args) {
         Ok(cli) => cli,
         Err(mut err) => {
+            let cmd = Cli::command().disable_colored_help(no_color);
+            err = err.with_cmd(&cmd);
             if let Some(ContextValue::String(subcommand)) = err.get(ContextKind::InvalidSubcommand)
             {
                 match subcommand.as_str() {
```

---

_Comment by @blueraft on 2024-09-10 07:40_

Awesome, thank you @eth3lbert. 

---

_Comment by @konstin on 2025-04-28 12:43_

I've rebased and comfirmed the change still works, anything missing here?

---

_Comment by @blueraft on 2025-04-28 12:59_

I don't think so, just waiting for review

---

_Review requested from @zanieb by @konstin on 2025-04-28 12:59_

---
