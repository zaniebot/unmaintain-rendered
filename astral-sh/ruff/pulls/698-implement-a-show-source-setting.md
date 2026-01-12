```yaml
number: 698
title: "Implement a `--show-source` setting"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: show-source
created_at: 2022-11-12T11:42:25Z
updated_at: 2022-11-19T01:50:57Z
url: https://github.com/astral-sh/ruff/pull/698
synced_at: 2026-01-12T05:48:45Z
```

# Implement a `--show-source` setting

---

_Pull request opened by @harupy on 2022-11-12 11:42_

See #525. 

```
cargo run -- --no-cache --select C resources/test/fixtures/ --show-source
```

<img width="1115" alt="image" src="https://user-images.githubusercontent.com/17039389/201511165-a0528f57-fbf8-4d72-b5dd-47068338d908.png">


---

_Renamed from "POC of `--show`source`" to "POC of `--show-source`" by @harupy on 2022-11-12 11:42_

---

_@harupy reviewed on 2022-11-12 11:43_

---

_Review comment by @harupy on `Cargo.toml`:16 on 2022-11-12 11:43_

rustc also uses this crate: https://github.com/rust-lang/rust/issues/59346#issuecomment-1279325650

---

_Renamed from "POC of `--show-source`" to "Add `--show-source` flag" by @harupy on 2022-11-12 11:45_

---

_@harupy reviewed on 2022-11-12 11:57_

---

_Review comment by @harupy on `Cargo.toml`:16 on 2022-11-12 11:57_

Another option is https://github.com/zkat/miette. SWC uses this crate to show pretty reports.

- https://github.com/swc-project/swc/pull/3946
- https://github.com/swc-project/swc/blob/27225a0eeb78f3c2975ec14ef51127b2b9532a84/crates/swc_error_reporters/Cargo.toml#L16

![image](https://user-images.githubusercontent.com/17039389/201473776-532bc1b2-e3be-4587-bcd6-46a24c32956a.png)


---

_Renamed from "Add `--show-source` flag" to "Add `--show-source` setting" by @harupy on 2022-11-12 12:00_

---

_Renamed from "Add `--show-source` setting" to "Implement a `--show-source` setting" by @harupy on 2022-11-12 12:00_

---

_Comment by @charliermarsh on 2022-11-12 14:10_

\cc @squiddy 

---

_Comment by @charliermarsh on 2022-11-12 14:12_

Wow nice. Is it possible to include the fix annotations / suggestions here too, like Clippy does?

---

_Comment by @harupy on 2022-11-12 14:16_

That's what I'm investigating now :)

---

_@charliermarsh reviewed on 2022-11-12 14:18_

---

_Review comment by @charliermarsh on `Cargo.toml`:16 on 2022-11-12 14:18_

It looks like that uses https://crates.io/crates/ariadne under the hood.

---

_Comment by @charliermarsh on 2022-11-12 16:40_

I do like the look and feel! And I love not implementing this ourselves.

---

_Comment by @harupy on 2022-11-13 00:43_

@charliermarsh It looks like clippy relies on `rustc_errors` (rustc's private crate) for creating an error report.

---

_@harupy reviewed on 2022-11-13 00:46_

---

_Review comment by @harupy on `src/message.rs`:116 on 2022-11-13 00:46_

This is where we can show notes or help messages:

```diff
-            footer: vec![],
+            footer: vec![Annotation {
+                label: Some("Add --fix to automatically fix this error"),
+                id: None,
+                annotation_type: AnnotationType::Help,
+            }],
```

<img width="520" alt="image" src="https://user-images.githubusercontent.com/17039389/201500272-700df862-9541-4962-ad80-6ac4c09328bd.png">


---

_Comment by @charliermarsh on 2022-11-13 03:30_

Could we vendor that crate? Or is it too closely coupled to Clippy / rustc to be extracted?

---

_Comment by @harupy on 2022-11-13 04:32_

This is `rustc_errors`' `Cargo.toml`:
https://github.com/rust-lang/rust/blob/fb6667a233fa677f6fbfa6067913ef3d32480317/compiler/rustc_errors/Cargo.toml

As you can see, it depends on other private crates:

```toml
[package]
name = "rustc_errors"
version = "0.0.0"
edition = "2021"

[lib]

[dependencies]
tracing = "0.1"
rustc_ast = { path = "../rustc_ast" }
rustc_ast_pretty = { path = "../rustc_ast_pretty" }
rustc_error_messages = { path = "../rustc_error_messages" }
rustc_serialize = { path = "../rustc_serialize" }
rustc_span = { path = "../rustc_span" }
rustc_macros = { path = "../rustc_macros" }
rustc_data_structures = { path = "../rustc_data_structures" }
rustc_target = { path = "../rustc_target" }
rustc_hir = { path = "../rustc_hir" }
rustc_lint_defs = { path = "../rustc_lint_defs" }
unicode-width = "0.1.4"
atty = "0.2"
termcolor = "1.0"
annotate-snippets = "0.9"
termize = "0.1.1"
serde = { version = "1.0.125", features = [ "derive" ] }
serde_json = "1.0.59"

[target.'cfg(windows)'.dependencies]
winapi = { version = "0.3", features = [ "handleapi", "synchapi", "winbase" ] }
```


---

_@charliermarsh reviewed on 2022-11-13 05:11_

---

_Review comment by @charliermarsh on `src/message.rs`:67 on 2022-11-13 05:11_

Can we find a way to do all of the above through the cached `SourceCodeLocator` that we already create for each file? (Maybe it involves adding the body text to `message` or something?)

---

_@charliermarsh reviewed on 2022-11-13 05:12_

---

_Review comment by @charliermarsh on `src/message.rs`:118 on 2022-11-13 05:12_

I think that if we're going to use this crate for `--show-source`, we should probably use the same crate and style even when `--show-source` is false (and just omit the source code). What do you think?

---

_Review comment by @harupy on `src/message.rs`:67 on 2022-11-13 05:54_

> Maybe it involves adding the body text to message or something?

I'll try this.

---

_@harupy reviewed on 2022-11-13 05:54_

---

_@harupy reviewed on 2022-11-13 05:55_

---

_Review comment by @harupy on `src/message.rs`:118 on 2022-11-13 05:55_

Sounds good. I think we can set `slices` to an empty vector when `--show-source` is false.

---

_Marked ready for review by @harupy on 2022-11-13 07:34_

---

_Comment by @harupy on 2022-11-13 07:35_

Updated the screenshot.

---

_Review requested from @charliermarsh by @harupy on 2022-11-14 01:15_

---

_@charliermarsh reviewed on 2022-11-15 21:50_

---

_Review comment by @charliermarsh on `src/cli.rs`:99 on 2022-11-15 21:50_

I think we should add this to `pyproject.toml` too, wdyt?

---

_Review comment by @charliermarsh on `src/message.rs`:44 on 2022-11-15 21:51_

I think this needs to be `.chars().len()`, or we need to use the `Rope` in `locator` to get the start and end _character_ positions.

---

_@charliermarsh reviewed on 2022-11-15 21:51_

---

_@charliermarsh reviewed on 2022-11-15 21:58_

---

_Review comment by @charliermarsh on `src/message.rs`:92 on 2022-11-15 21:58_

Nit: maybe `if let` to avoid some of the unwraps?

---

_@harupy reviewed on 2022-11-16 02:12_

---

_Review comment by @harupy on `src/cli.rs`:99 on 2022-11-16 02:12_

Looks like I forgot to update `user.rs`. Updated it.

---

_@harupy reviewed on 2022-11-16 02:17_

---

_Review comment by @harupy on `src/message.rs`:92 on 2022-11-16 02:17_

Sounds good. Will fix.

---

_@harupy reviewed on 2022-11-16 02:18_

---

_Review comment by @harupy on `src/message.rs`:44 on 2022-11-16 02:18_

Fixed

---

_Review requested from @charliermarsh by @harupy on 2022-11-16 04:27_

---

_Comment by @charliermarsh on 2022-11-16 04:40_

Sweet! Will try to merge this tomorrow (or maybe the next day -- I have to move apartments tomorrow so might be busy). I just want to do some performance testing before merging.

---

_Comment by @charliermarsh on 2022-11-17 19:53_

(Sorry, I'm a little behind, I haven't had a chance to benchmark yet.)

---

_Comment by @harupy on 2022-11-18 04:14_

@charliermarsh Not a problem at all :)

---

_Merged by @charliermarsh on 2022-11-18 19:02_

---

_Closed by @charliermarsh on 2022-11-18 19:02_

---

_Comment by @harupy on 2022-11-19 01:29_

@charliermarsh Thanks for the clean-up and merge! This feature does help when verifying new lint rules.

---

_Branch deleted on 2022-11-19 01:50_

---
