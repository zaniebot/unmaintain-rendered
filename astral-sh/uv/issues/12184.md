```yaml
number: 12184
title: "Should `uv python install 3` install an alpha version?"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
assignees: []
created_at: 2025-03-15T10:18:58Z
updated_at: 2025-04-21T21:16:08Z
url: https://github.com/astral-sh/uv/issues/12184
synced_at: 2026-01-10T03:41:47Z
```

# Should `uv python install 3` install an alpha version?

---

_Issue opened by @bersbersbers on 2025-03-15 10:18_

### Summary

Because it does:

```
'uv' (0.6.6) was installed successfully!

>uv python install 3    
cpython-3.14.0a5-windows-x86_64-none ------------------------------ 2.11 MiB/20.26 MiB                                                            
```

### Platform

Windows 11

### Version

0.6.6

### Python version

_No response_

---

_Label `bug` added by @bersbersbers on 2025-03-15 10:18_

---

_Comment by @charliermarsh on 2025-03-15 14:52_

Hmm. Probably not, no.

---

_Comment by @zanieb on 2025-03-15 20:13_

Huh, that's surprising.

---

_Comment by @zanieb on 2025-03-15 20:25_

I think the logic isn't quite right

https://github.com/astral-sh/uv/blob/553bcccb6ae8139ecf1545854c0712961b978879/crates/uv-python/src/downloads.rs#L172-L178

https://github.com/astral-sh/uv/blob/553bcccb6ae8139ecf1545854c0712961b978879/crates/uv-python/src/downloads.rs#L277-L280

https://github.com/astral-sh/uv/blob/553bcccb6ae8139ecf1545854c0712961b978879/crates/uv-python/src/downloads.rs#L304-L311

https://github.com/astral-sh/uv/blob/3a53ec3c5af88a50fbb334a73880a41fd9e9b3a5/crates/uv-python/src/discovery.rs#L2273-L2284

We'll want to try to apply

```diff
diff --git a/crates/uv-python/src/discovery.rs b/crates/uv-python/src/discovery.rs
index b5a4972d6..1aa58c20e 100644
--- a/crates/uv-python/src/discovery.rs
+++ b/crates/uv-python/src/discovery.rs
@@ -2275,9 +2275,9 @@ impl VersionRequest {
         match self {
             Self::Default => false,
             Self::Any => true,
-            Self::Major(..) => true,
-            Self::MajorMinor(..) => true,
-            Self::MajorMinorPatch(..) => true,
+            Self::Major(..) => false,
+            Self::MajorMinor(..) => false,
+            Self::MajorMinorPatch(..) => false,
             Self::MajorMinorPrerelease(..) => true,
             Self::Range(specifiers, _) => specifiers.iter().any(VersionSpecifier::any_prerelease),
         }
```

but we still want `uv python install 3.14` to work so we'll need to implement logic for downloads like this existing pattern for discovery

https://github.com/astral-sh/uv/blob/3a53ec3c5af88a50fbb334a73880a41fd9e9b3a5/crates/uv-python/src/discovery.rs#L1100-L1103

---

_Assigned to @zanieb by @zanieb on 2025-03-15 20:26_

---

_Closed by @zanieb on 2025-04-21 21:16_

---

_Closed by @zanieb on 2025-04-21 21:16_

---
