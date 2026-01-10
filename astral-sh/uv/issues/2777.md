```yaml
number: 2777
title: No wheels found with matching Python ABI error message does not show ABI
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-04-02T14:19:51Z
updated_at: 2025-01-14T08:58:04Z
url: https://github.com/astral-sh/uv/issues/2777
synced_at: 2026-01-10T04:27:57Z
```

# No wheels found with matching Python ABI error message does not show ABI

---

_Issue opened by @zanieb on 2024-04-02 14:19_

e.g.  in

```
  ╰─▶ Because torch==2.1.0+cpu is unusable because no wheels are available with a matching Python ABI and you require torch==2.1.0+cpu, we can conclude that the requirements are unsatisfiable.
```

We don't say what the current ABI is that could not be satisfied — can we show this effectively?

---

_Label `error messages` added by @zanieb on 2024-04-02 14:19_

---

_Comment by @fzyzcjy on 2024-10-19 11:56_

Hi, is there any updates? Thanks!

---

_Comment by @charliermarsh on 2024-12-17 03:53_

I _think_ the actual representation here is a long list of tags... But maybe we can show some summary value?

---

_Comment by @zanieb on 2024-12-17 04:13_

I started doing some work to thread information through here... I think there are two parts:

- Which tags are compatible?
- Which tags are available?

I investigated attaching the tags available on the wheels to our incompatibility tracking system, but I don't remember how far I made it.

---

_Comment by @zanieb on 2024-12-17 15:35_

I found the change I started with

```diff
commit 55d61b8fab03bc520dd6ac0420f2d47d50269323
Author: Zanie Blue <contact@zanie.dev>
Date:   Sat Oct 19 08:38:13 2024 -0500

    Track the best incompatible ABI tag

diff --git a/crates/uv-distribution-types/src/prioritized_distribution.rs b/crates/uv-distribution-types/src/prioritized_distribution.rs
index 5a6aa2cd8..bc27516a6 100644
--- a/crates/uv-distribution-types/src/prioritized_distribution.rs
+++ b/crates/uv-distribution-types/src/prioritized_distribution.rs
@@ -138,7 +138,9 @@ impl Display for IncompatibleDist {
                     IncompatibleTag::Python => {
                         f.write_str("no wheels with a matching Python implementation tag")
                     }
-                    IncompatibleTag::Abi => f.write_str("no wheels with a matching Python ABI tag"),
+                    IncompatibleTag::Abi(abi) => {
+                        f.write_str(&format!("no wheels with a matching Python ABI tag ({abi})"))
+                    }
                     IncompatibleTag::Platform => {
                         f.write_str("no wheels with a matching platform tag")
                     }
diff --git a/crates/uv-platform-tags/src/tags.rs b/crates/uv-platform-tags/src/tags.rs
index ae4f702d5..e93884c19 100644
--- a/crates/uv-platform-tags/src/tags.rs
+++ b/crates/uv-platform-tags/src/tags.rs
@@ -25,7 +25,7 @@ pub enum TagsError {
 pub enum IncompatibleTag {
     Invalid,
     Python,
-    Abi,
+    Abi(String),
     Platform,
 }
 
@@ -246,8 +246,9 @@ impl Tags {
             };
             for wheel_abi in wheel_abi_tags {
                 let Some(platforms) = abis.get(wheel_abi) else {
-                    max_compatibility =
-                        max_compatibility.max(TagCompatibility::Incompatible(IncompatibleTag::Abi));
+                    max_compatibility = max_compatibility.max(TagCompatibility::Incompatible(
+                        IncompatibleTag::Abi(wheel_abi.clone()),
+                    ));
                     continue;
                 };
                 for wheel_platform in wheel_platform_tags {
diff --git a/crates/uv-resolver/src/version_map.rs b/crates/uv-resolver/src/version_map.rs
index c54b472df..2711c3de8 100644
--- a/crates/uv-resolver/src/version_map.rs
+++ b/crates/uv-resolver/src/version_map.rs
@@ -1,3 +1,4 @@
+use itertools::Itertools;
 use pubgrub::Range;
 use std::collections::btree_map::{BTreeMap, Entry};
 use std::sync::OnceLock;
@@ -563,7 +564,9 @@ impl VersionMapLazy {
         // Check if the wheel is compatible with the `requires-python` (i.e., the Python ABI tag
         // is not less than the `requires-python` minimum version).
         if !self.requires_python.matches_wheel_tag(filename) {
-            return WheelCompatibility::Incompatible(IncompatibleWheel::Tag(IncompatibleTag::Abi));
+            return WheelCompatibility::Incompatible(IncompatibleWheel::Tag(IncompatibleTag::Abi(
+                filename.abi_tag.iter().join("."),
+            )));
         }
 
         // Break ties with the build tag.
diff --git a/crates/uv/tests/it/lock.rs b/crates/uv/tests/it/lock.rs
index cc1b0c35d..14f5e734f 100644
--- a/crates/uv/tests/it/lock.rs
+++ b/crates/uv/tests/it/lock.rs
@@ -5521,7 +5521,7 @@ fn lock_requires_python_no_wheels() -> Result<()> {
 
     ----- stderr -----
       × No solution found when resolving dependencies:
-      ╰─▶ Because dearpygui==1.9.1 has no wheels with a matching Python ABI tag and your project depends on dearpygui==1.9.1, we can conclude that your project's requirements are unsatisfiable.
+      ╰─▶ Because dearpygui==1.9.1 has no wheels with a matching Python ABI tag (cp310) and your project depends on dearpygui==1.9.1, we can conclude that your project's requirements are unsatisfiable.
     "###);
 
     Ok(())
diff --git a/crates/uv/tests/it/pip_install_scenarios.rs b/crates/uv/tests/it/pip_install_scenarios.rs
index 7116d2b4f..f028d9dd6 100644
--- a/crates/uv/tests/it/pip_install_scenarios.rs
+++ b/crates/uv/tests/it/pip_install_scenarios.rs
@@ -4267,16 +4267,16 @@ fn no_sdist_no_wheels_with_matching_abi() {
 
     uv_snapshot!(filters, command(&context)
         .arg("no-sdist-no-wheels-with-matching-abi-a")
-        , @r#"
+        , @r###"
     success: false
     exit_code: 1
     ----- stdout -----
 
     ----- stderr -----
       × No solution found when resolving dependencies:
-      ╰─▶ Because only package-a==1.0.0 is available and package-a==1.0.0 has no wheels with a matching Python ABI tag, we can conclude that all versions of package-a cannot be used.
+      ╰─▶ Because only package-a==1.0.0 is available and package-a==1.0.0 has no wheels with a matching Python ABI tag (MMMMMM), we can conclude that all versions of package-a cannot be used.
           And because you require package-a, we can conclude that your requirements are unsatisfiable.
-    "#);
+    "###);
 
     assert_not_installed(
         &context.venv,

```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 02:56_

---

_Comment by @charliermarsh on 2025-01-09 18:24_

Gonna look at this today.

---

_Comment by @konstin on 2025-01-13 21:33_

My high-level comment is that it would be nice if we could show the user the msimatch in categories of python version, platform and architecture, if only one of them mismatches:

> A wheel exists for a different python version (current: `cp313`, supported: `cp311`, `cp310`, `cp309`)

Or

> A wheel exists for a different platform (current: MacOS, supported: Linux, Windows)

Or

> A wheel exists for a different architecture (current: arm64, supported: x86_86)

In my experience, it is usually one of those that mismatches. I've had version mismatches when e.g. i was using 3.13 while up until 3.12 was supported. Torch is a good example for platform support problems, and the arm64/x86_64 is probably something that happens with the mac architecture transition or when you are on a less popular architecture (e.g. a raspberry pi or specific servers or devboards). This comes with the other motive of being clearer about build failures, the "a wheel exists for a different python version" can tell the user to downgrade without them having to figure out the files page and pypi and how to read wheel tags.

---

_Comment by @charliermarsh on 2025-01-13 21:34_

@konstin -- Did you look at #10534?

---

_Comment by @charliermarsh on 2025-01-13 21:34_

That's what you're asking for, but doesn't put it in plaintext as nicely.

---

_Comment by @konstin on 2025-01-13 21:36_

Yes, i just didn't want to make it a PR review comment (this could be done in a follow-up/later?) so i put it here to get to a shared target output.

---

_Comment by @charliermarsh on 2025-01-13 22:47_

(@konstin -- Just clarifying that https://github.com/astral-sh/uv/pull/10534 is different than https://github.com/astral-sh/uv/pull/10527 which you reviewed.)

---

_Closed by @charliermarsh on 2025-01-14 01:03_

---

_Closed by @charliermarsh on 2025-01-14 01:03_

---

_Comment by @zhangrongyu0101 on 2025-01-14 08:58_

maybe it's the problem with the Python version? I solved this problem by changing the Python version from 3.13 to 3.10.


---
