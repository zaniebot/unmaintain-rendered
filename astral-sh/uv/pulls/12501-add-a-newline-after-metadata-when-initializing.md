```yaml
number: 12501
title: Add a newline after metadata when initializing scripts with other metadata blocks
type: pull_request
state: merged
author: merlinz01
labels:
  - bug
assignees: []
merged: true
base: main
head: add-newline-after-metadata
created_at: 2025-03-27T01:46:38Z
updated_at: 2025-03-27T21:44:52Z
url: https://github.com/astral-sh/uv/pull/12501
synced_at: 2026-01-10T11:10:40Z
```

# Add a newline after metadata when initializing scripts with other metadata blocks

---

_Pull request opened by @merlinz01 on 2025-03-27 01:46_

## Summary

uv doesn't separate the metadata block from other blocks when adding the `script` block to a script, which results in the next block being considered part of the script block and causes errors when running.

See #12499 for more details.

Closes #12499

## Test Plan

I manually tested the most common scenario, but there's a few edge cases that would be good to have tests for.

I would have written the tests also, but I was running into errors like this:
```bash
$ cargo test --package uv-scripts
   Compiling uv-configuration v0.0.1 (/home/merlin/Projects/uv/crates/uv-configuration)
error: cannot find attribute `value` in this scope
 --> crates/uv-configuration/src/project_build_backend.rs:8:38
  |
8 |     #[cfg_attr(feature = "schemars", value(hide = true))]
  |                                      ^^^^^

error: could not compile `uv-configuration` (lib) due to 1 previous error
```

---

_Comment by @charliermarsh on 2025-03-27 02:14_

Thanks, a few tests would be great. You can apply this patch:

```diff
diff --git a/crates/uv-configuration/src/project_build_backend.rs b/crates/uv-configuration/src/project_build_backend.rs
index fb1a171e9..90fac2ec9 100644
--- a/crates/uv-configuration/src/project_build_backend.rs
+++ b/crates/uv-configuration/src/project_build_backend.rs
@@ -5,7 +5,7 @@
 #[cfg_attr(feature = "schemars", derive(schemars::JsonSchema))]
 pub enum ProjectBuildBackend {
     #[cfg_attr(feature = "clap", value(hide = true))]
-    #[cfg_attr(feature = "schemars", value(hide = true))]
+    #[cfg_attr(feature = "schemars", schemars(skip))]
     /// Use uv as the project build backend.
     Uv,
     #[default]
```

---

_Comment by @charliermarsh on 2025-03-27 02:21_

Ah, looks like you also need https://github.com/astral-sh/uv/pull/12504.

---

_Comment by @merlinz01 on 2025-03-27 02:21_

Hmm, now I'm getting lots of errors like this:
```bash
$ cargo test --package uv-scripts
   Compiling uv-settings v0.0.1 (/home/merlin/Projects/uv/crates/uv-settings)
error[E0277]: the trait bound `PythonPreference: ValueEnum` is not satisfied
   --> crates/uv-settings/src/settings.rs:252:35
    |
252 |     pub python_preference: Option<PythonPreference>,
    |                                   ^^^^^^^^^^^^^^^^ the trait `ValueEnum` is not implemented for `PythonPreference`
    |
    = help: the trait `ValueEnum` is implemented for `clap::ColorChoice`
# 25 more similar errors...
```

This is what I'm running:
```bash
$ cargo --version
cargo 1.85.1 (d73d2caf9 2024-12-31)
$ rustup --version
rustup 1.26.0 (2024-04-01)
info: This is the version for the rustup toolchain manager, not the rustc compiler.
info: The currently active `rustc` version is `rustc 1.85.1 (4eb161250 2025-03-15)`
$ rustc --version
rustc 1.85.1 (4eb161250 2025-03-15)
$ uname -a
Linux myhostname 6.8.0-55-generic #57-Ubuntu SMP PREEMPT_DYNAMIC Wed Feb 12 23:42:21 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
$ cat /etc/os-release 
NAME="Linux Mint"
VERSION="22.1 (Xia)"
```

---

_Comment by @charliermarsh on 2025-03-27 02:24_

Those PRs should merge shortly, then you can rebase on top of them if you'd like. (The problem, I think, is that we tend to run `cargo test` over the entire workspace, so as long as one crate enables the necessary features, all crates have access. Thus, you only see these failures if you try to test a crate individually.)

---

_Comment by @merlinz01 on 2025-03-27 17:10_

Thanks for the speedy fix!  I have added tests.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-27 19:40_

---

_@charliermarsh reviewed on 2025-03-27 19:51_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:1038 on 2025-03-27 19:51_

It _might_ be easier to do this via snapshot testing?

---

_@merlinz01 reviewed on 2025-03-27 21:14_

---

_Review comment by @merlinz01 on `crates/uv-scripts/src/lib.rs`:1038 on 2025-03-27 21:14_

I can't find how to import the `uv_snapshot` macro.

---

_@charliermarsh approved on 2025-03-27 21:22_

Thanks.

---

_@charliermarsh reviewed on 2025-03-27 21:22_

---

_Review comment by @charliermarsh on `crates/uv-scripts/src/lib.rs`:1038 on 2025-03-27 21:22_

I looked into it briefly but I think `insta` strips some whitespace, and these tests are intentionally quite whitespace-sensitive, so lets leave it as-is.

---

_Label `bug` added by @charliermarsh on 2025-03-27 21:23_

---

_Merged by @charliermarsh on 2025-03-27 21:39_

---

_Closed by @charliermarsh on 2025-03-27 21:39_

---

_Branch deleted on 2025-03-27 21:44_

---
