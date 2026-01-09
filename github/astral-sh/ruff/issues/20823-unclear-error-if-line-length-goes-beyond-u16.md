---
number: 20823
title: Unclear error if line-length goes beyond u16 boundaries
type: issue
state: closed
author: chirizxc
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-10-12T20:56:46Z
updated_at: 2025-10-27T07:42:50Z
url: https://github.com/astral-sh/ruff/issues/20823
synced_at: 2026-01-07T13:12:16-06:00
---

# Unclear error if line-length goes beyond u16 boundaries

---

_Issue opened by @chirizxc on 2025-10-12 20:56_

### Summary

<img width="1507" height="329" alt="Image" src="https://github.com/user-attachments/assets/deab4342-14c5-4e66-9930-8cc44349d786" />

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Comment by @chirizxc on 2025-10-12 21:16_

I would expect something like:

<img width="1527" height="266" alt="Image" src="https://github.com/user-attachments/assets/e37cb150-ae94-41ed-98fd-c43ca419b8f0" />

```diff
❯ git diff
diff --git a/crates/ruff_linter/src/line_width.rs b/crates/ruff_linter/src/line_width.rs
index 7525a68cd..b9d721e71 100644
--- a/crates/ruff_linter/src/line_width.rs
+++ b/crates/ruff_linter/src/line_width.rs
@@ -14,7 +14,7 @@ use ruff_text_size::TextSize;
 /// The length of a line of text that is considered too long.
 ///
 /// The allowed range of values is 1..=320
-#[derive(Clone, Copy, Debug, Eq, PartialEq, serde::Serialize, serde::Deserialize)]
+#[derive(Clone, Copy, Debug, Eq, PartialEq, serde::Serialize)]
 #[cfg_attr(feature = "schemars", derive(schemars::JsonSchema))]
 pub struct LineLength(
     #[cfg_attr(feature = "schemars", schemars(range(min = 1, max = 320)))] NonZeroU16,
@@ -112,6 +112,21 @@ impl std::fmt::Display for LineLengthFromIntError {
     }
 }
 
+impl<'de> serde::Deserialize<'de> for LineLength {
+    fn deserialize<D>(deserializer: D) -> Result<Self, D::Error>
+    where
+        D: serde::Deserializer<'de>,
+    {
+        let value = u64::deserialize(deserializer)?;
+        let value = u16::try_from(value)
+            .map_err(|_| serde::de::Error::custom(
+                format!("line-length must be between 1 and {} (got {})", Self::MAX, value)
+            ))?;
+
+        Self::try_from(value).map_err(serde::de::Error::custom)
+    }
+}
+
 impl From<LineLength> for u16 {
     fn from(value: LineLength) -> Self {
         value.0.get()
```

---

_Comment by @chirizxc on 2025-10-12 21:22_

I don't really know where numbers larger than `65535` can occur on real projects

---

_Label `help wanted` added by @MichaReiser on 2025-10-13 06:30_

---

_Label `diagnostics` added by @MichaReiser on 2025-10-13 06:30_

---

_Referenced in [astral-sh/ruff#21072](../../astral-sh/ruff/pulls/21072.md) on 2025-10-25 10:54_

---

_Closed by @MichaReiser on 2025-10-27 07:42_

---

_Referenced in [astral-sh/ruff#21328](../../astral-sh/ruff/issues/21328.md) on 2025-11-07 22:44_

---
