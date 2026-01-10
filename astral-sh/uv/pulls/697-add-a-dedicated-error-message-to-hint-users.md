```yaml
number: 697
title: Add a dedicated error message to hint users towards enabling pre-releases
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/errr
created_at: 2023-12-19T05:35:04Z
updated_at: 2023-12-29T02:44:36Z
url: https://github.com/astral-sh/uv/pull/697
synced_at: 2026-01-10T15:44:44Z
```

# Add a dedicated error message to hint users towards enabling pre-releases

---

_Pull request opened by @charliermarsh on 2023-12-19 05:35_

This PR adds a dedicated error message for resolutions that fail, but might've succeeded if pre-releases were allowed. Specifically, if we see a failed resolution, and failed to find a version for a package that included a pre-release marker, we add a hint nudging the user to explicitly enable all pre-releases.

We'd prefer a solution like https://github.com/astral-sh/puffin/pull/666, but believe that it will break some assumptions in PubGrub, so this is the lighter-weight solution.

Closes https://github.com/astral-sh/puffin/issues/659.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-19 05:35_

---

_Review requested from @konstin by @charliermarsh on 2023-12-19 05:35_

---

_@charliermarsh reviewed on 2023-12-19 05:35_

---

_Review comment by @charliermarsh on `Cargo.toml`:54 on 2023-12-19 05:35_

This requires a change in PubGrub, separate PR:

```diff
diff --git a/src/range.rs b/src/range.rs
index fc04e8b..b817f23 100644
--- a/src/range.rs
+++ b/src/range.rs
@@ -122,6 +122,23 @@ impl<V> Range<V> {
     pub fn is_empty(&self) -> bool {
         self.segments.is_empty()
     }
+
+    pub fn bounds(&self) -> impl Iterator<Item = &V> {
+        self.segments.iter().flat_map(|segment| {
+            let (v1, v2) = segment;
+            let v1 = match v1 {
+                Included(v) => Some(v),
+                Excluded(v) => Some(v),
+                Unbounded => None
+            };
+            let v2 = match v2 {
+                Included(v) => Some(v),
+                Excluded(v) => Some(v),
+                Unbounded => None
+            };
+            v1.into_iter().chain(v2)
+        })
+    }
 }
```

---

_@konstin reviewed on 2023-12-19 09:00_

I don't know if we can guarantee that we hit this path, but it's definitely helpful where we hit that path.

Needs a pubgrub update, otherwise we can merge it.

---

_@charliermarsh reviewed on 2023-12-24 21:48_

---

_Review comment by @charliermarsh on `Cargo.toml`:54 on 2023-12-24 21:48_

@zanieb - I PR'd this here: https://github.com/zanieb/pubgrub/pull/16

---

_@zanieb approved on 2023-12-28 17:53_

---

_Merged by @charliermarsh on 2023-12-29 02:44_

---

_Closed by @charliermarsh on 2023-12-29 02:44_

---

_Branch deleted on 2023-12-29 02:44_

---
