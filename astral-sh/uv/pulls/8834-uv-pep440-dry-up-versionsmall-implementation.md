```yaml
number: 8834
title: "uv-pep440: DRY up VersionSmall implementation"
type: pull_request
state: merged
author: BurntSushi
labels:
  - rustlib
assignees: []
merged: true
base: main
head: ag/small-version-tweaks
created_at: 2024-11-05T17:56:54Z
updated_at: 2024-11-05T18:26:21Z
url: https://github.com/astral-sh/uv/pull/8834
synced_at: 2026-01-12T16:08:31Z
```

# uv-pep440: DRY up VersionSmall implementation

---

_@BurntSushi_

This PR simplifies the VersionSmall implementation a bit by utilizing
more constants. That is, if the bit-level format changes, *most* of
those changes should be implementable by just changing the constants.
Previously, you would need to audit and tweak the code as well. (The
exception here is `push_release`. If the release segment bit format is
changed, then that function will need to be tweaked. I didn't think it
was worth over-complicating things to make its implementation more
general.)


---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-05 17:57_

---

_Comment by @BurntSushi on 2024-11-05 17:57_

With this PR, to add a "local" suffix kind, I believe this is all that's needed (at least, for making changes to the bit-level representation):

```
$ git diff
diff --git a/crates/uv-pep440/src/version.rs b/crates/uv-pep440/src/version.rs
index 22b9730ed..6e51df393 100644
--- a/crates/uv-pep440/src/version.rs
+++ b/crates/uv-pep440/src/version.rs
@@ -922,22 +922,27 @@ impl VersionSmall {
     const SUFFIX_PRE_BETA: u64 = 3;
     const SUFFIX_PRE_RC: u64 = 4;
     const SUFFIX_NONE: u64 = 5;
-    const SUFFIX_POST: u64 = 6;
-    const SUFFIX_MAX: u64 = 7;
+    const SUFFIX_LOCAL: u64 = 6;
+    const SUFFIX_POST: u64 = 7;
+    const SUFFIX_MAX: u64 = 8;

     // The mask to get only the release segment bits.
     const SUFFIX_RELEASE_MASK: u64 = 0xFFFF_FFFF_FF00_0000;
     // The mask to get the version suffix.
-    const SUFFIX_VERSION_MASK: u64 = 0x001F_FFFF;
+    const SUFFIX_VERSION_MASK: u64 = 0x000F_FFFF;
     // The number of bits used by the version suffix. Shifting the `repr`
     // right by this number of bits should put the suffix kind in the least
     // significant bits.
-    const SUFFIX_VERSION_BIT_LEN: u64 = 21;
+    const SUFFIX_VERSION_BIT_LEN: u64 = 20;
     // The mask to get only the suffix kind, after shifting right by the
     // version bits. If you need to add a bit here, then you'll probably need
     // to take a bit from the suffix version. (Which requires a change to both
     // the mask and the bit length above.)
-    const SUFFIX_KIND_MASK: u64 = 0b111;
+    //
+    // NOTE: If you do change the bit format here, you'll need to bump any
+    // cache versions in uv that use rkyv with `Version` in them. That includes
+    // *at least* the "simple" cache.
+    const SUFFIX_KIND_MASK: u64 = 0b1111;

     #[inline]
     fn new() -> Self {
```

---

_Comment by @charliermarsh on 2024-11-05 17:58_

Thank you! 🙏 

---

_@charliermarsh approved on 2024-11-05 17:59_

Thanks so helpful.

---

_Label `rustlib` added by @zanieb on 2024-11-05 18:04_

---

_Merged by @BurntSushi on 2024-11-05 18:26_

---

_Closed by @BurntSushi on 2024-11-05 18:26_

---

_Branch deleted on 2024-11-05 18:26_

---
