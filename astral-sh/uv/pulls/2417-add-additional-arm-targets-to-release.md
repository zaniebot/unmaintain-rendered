```yaml
number: 2417
title: Add additional ARM targets to release
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/arm
created_at: 2024-03-13T14:49:34Z
updated_at: 2024-03-15T13:49:30Z
url: https://github.com/astral-sh/uv/pull/2417
synced_at: 2026-01-10T14:49:08Z
```

# Add additional ARM targets to release

---

_Pull request opened by @charliermarsh on 2024-03-13 14:49_

Closes https://github.com/astral-sh/uv/issues/2415.
Closes https://github.com/astral-sh/uv/issues/2416.

---

_Comment by @charliermarsh on 2024-03-13 14:50_

This will take some trial and error.

---

_Label `release` added by @charliermarsh on 2024-03-13 16:07_

---

_Comment by @messense on 2024-03-14 06:15_

FYI, I've just added armv6 musl target support in https://github.com/PyO3/maturin-action/pull/250, if you try again without the `before-script-linux` changes, it might just work.

---

_Comment by @charliermarsh on 2024-03-14 13:06_

Woah, was that just a coincidence? Amazing. I’ll try it today.

---

_Review requested from @konstin by @charliermarsh on 2024-03-14 18:15_

---

_Marked ready for review by @charliermarsh on 2024-03-14 18:15_

---

_@charliermarsh reviewed on 2024-03-14 18:32_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:538 on 2024-03-14 18:32_

Should we build this here, or in the `linux-arm` category that I augmented above?

---

_@charliermarsh reviewed on 2024-03-14 20:56_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/platform.rs`:113 on 2024-03-14 20:56_

@konstin - This is a bit awkward

---

_Comment by @messense on 2024-03-15 02:40_

> Woah, was that just a coincidence?

I saw this PR and thought that it can be supported directly in maturin-action.

---

_Comment by @charliermarsh on 2024-03-15 02:41_

@messense - Thank you very much.

---

_@messense reviewed on 2024-03-15 02:42_

---

_Review comment by @messense on `.github/workflows/build-binaries.yml`:309 on 2024-03-15 02:42_

In fact, `linux_armv6l` wheels are permited to upload to PyPI at the moment.

See https://github.com/pypi/warehouse/issues/3668

---

_@charliermarsh reviewed on 2024-03-15 02:43_

---

_Review comment by @charliermarsh on `.github/workflows/build-binaries.yml`:309 on 2024-03-15 02:43_

Oh wow. Maturin seems to warn on this though: "Warning: No compatible platform tag found, using the linux tag instead. You won't be able to upload those wheels to PyPI."

---

_@messense reviewed on 2024-03-15 02:46_

---

_Review comment by @messense on `.github/workflows/build-binaries.yml`:309 on 2024-03-15 02:46_

Yeah the warning is intentional, because there is no official manylinux/musllinux policy for armv6l and https://github.com/pypi/warehouse/issues/3668 were talking about removing the support.

---

_Review comment by @konstin on `crates/platform-tags/src/platform.rs`:113 on 2024-03-15 08:25_

What about

```diff
diff --git a/crates/platform-tags/src/platform.rs b/crates/platform-tags/src/platform.rs
index c28e9b76..d36c91e9 100644
--- a/crates/platform-tags/src/platform.rs
+++ b/crates/platform-tags/src/platform.rs
@@ -103,14 +103,16 @@ impl fmt::Display for Arch {
 
 impl Arch {
     /// Returns the oldest possible Manylinux tag for this architecture
-    pub fn get_minimum_manylinux_minor(&self) -> u16 {
+    pub fn get_minimum_manylinux_minor(&self) -> Option<u16> {
         match self {
             // manylinux 2014
-            Self::Aarch64 | Self::Armv7L | Self::Powerpc64 | Self::Powerpc64Le | Self::S390X => 17,
+            Self::Aarch64 | Self::Armv7L | Self::Powerpc64 | Self::Powerpc64Le | Self::S390X => {
+                Some(17)
+            }
             // manylinux 1
-            Self::X86 | Self::X86_64 => 5,
+            Self::X86 | Self::X86_64 => Some(5),
             // not supported
-            Self::Armv6L => 0,
+            Self::Armv6L => None,
         }
     }
 }
diff --git a/crates/platform-tags/src/tags.rs b/crates/platform-tags/src/tags.rs
index b4db57da..3cd9a2db 100644
--- a/crates/platform-tags/src/tags.rs
+++ b/crates/platform-tags/src/tags.rs
@@ -336,18 +336,19 @@ fn compatible_tags(platform: &Platform) -> Result<Vec<String>, PlatformError> {
     let platform_tags = match (&os, arch) {
         (Os::Manylinux { major, minor }, _) => {
             let mut platform_tags = vec![format!("linux_{}", arch)];
-            platform_tags.extend(
-                (arch.get_minimum_manylinux_minor()..=*minor)
-                    .map(|minor| format!("manylinux_{major}_{minor}_{arch}")),
-            );
-            if (arch.get_minimum_manylinux_minor()..=*minor).contains(&12) {
-                platform_tags.push(format!("manylinux2010_{arch}"));
-            }
-            if (arch.get_minimum_manylinux_minor()..=*minor).contains(&17) {
-                platform_tags.push(format!("manylinux2014_{arch}"));
-            }
-            if (arch.get_minimum_manylinux_minor()..=*minor).contains(&5) {
-                platform_tags.push(format!("manylinux1_{arch}"));
+            if let Some(min_minor) = arch.get_minimum_manylinux_minor() {
+                platform_tags.extend(
+                    (min_minor..=*minor).map(|minor| format!("manylinux_{major}_{minor}_{arch}")),
+                );
+                if (min_minor..=*minor).contains(&12) {
+                    platform_tags.push(format!("manylinux2010_{arch}"));
+                }
+                if (min_minor..=*minor).contains(&17) {
+                    platform_tags.push(format!("manylinux2014_{arch}"));
+                }
+                if (min_minor..=*minor).contains(&5) {
+                    platform_tags.push(format!("manylinux1_{arch}"));
+                }
             }
             platform_tags
         }
```

---

_@konstin approved on 2024-03-15 08:26_

---

_Merged by @charliermarsh on 2024-03-15 13:49_

---

_Closed by @charliermarsh on 2024-03-15 13:49_

---

_Branch deleted on 2024-03-15 13:49_

---
