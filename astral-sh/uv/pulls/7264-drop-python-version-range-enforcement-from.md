```yaml
number: 7264
title: "Drop Python version range enforcement from `PythonVersion::from_str`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/python-version-enforcement
created_at: 2024-09-10T17:42:25Z
updated_at: 2024-09-10T18:34:19Z
url: https://github.com/astral-sh/uv/pull/7264
synced_at: 2026-01-12T16:07:45Z
```

# Drop Python version range enforcement from `PythonVersion::from_str`

---

_@zanieb_

This caused some problems earlier, as it prevented us from _listing_ Python versions <3.7 which seems weird (see https://github.com/astral-sh/uv/pull/7131#issuecomment-2334929000)

I'm worried that without this the changes to installation key parsing in https://github.com/astral-sh/uv/pull/7263 would otherwise be too restrictive.

I think if we want to enforce these ranges, we should do so separately from the parse step.

---

_Comment by @zanieb on 2024-09-10 18:02_

@konstin looking to add this validation somewhere but not sure where it belongs

It looks like this error variant is never used?

https://github.com/astral-sh/uv/blob/4f2349119cf341eedf738d06a50ed136a5f207db/crates/uv-python/src/interpreter.rs#L560-L561

We probably need to check this during version requests too?

For future reference, explicit validation looks like this

```diff
diff --git a/crates/uv-python/src/python_version.rs b/crates/uv-python/src/python_version.rs
index 6c4a99487..db83bb4ac 100644
--- a/crates/uv-python/src/python_version.rs
+++ b/crates/uv-python/src/python_version.rs
@@ -165,6 +165,18 @@ impl PythonVersion {
         Self::from_str(format!("{}.{}", self.major(), self.minor()).as_str())
             .expect("dropping a patch should always be valid")
     }
+
+    /// Returns an error if the Python version is not supported.
+    pub fn check_supported(&self) -> Result<(), String> {
+        if version.version < Version::new([3, 7]) {
+            return Err(format!("Python version `{s}` must be >= 3.7"));
+        }
+        if version.version >= Version::new([4, 0]) {
+            return Err(format!("Python version `{s}` must be < 4.0"));
+        }
+
+        Ok(())
+    }
 }
 
 #[cfg(test)]
```


---

_Marked ready for review by @zanieb on 2024-09-10 18:15_

---

_Comment by @konstin on 2024-09-10 18:22_

We should be filtering out too old interpreters at https://github.com/astral-sh/uv/blob/c9774e9c435b49e19cba54c6bdeb458849a7f5cd/crates/uv-python/python/get_interpreter_info.py#L446

---

_@konstin approved on 2024-09-10 18:23_

---

_Comment by @zanieb on 2024-09-10 18:34_

Ah okay so it's constructed by the deserializer. Lgtm then.

---

_Merged by @zanieb on 2024-09-10 18:34_

---

_Closed by @zanieb on 2024-09-10 18:34_

---

_Branch deleted on 2024-09-10 18:34_

---
