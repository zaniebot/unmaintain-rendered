---
number: 360
title: False positive for W292
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2022-10-08T03:05:20Z
updated_at: 2022-10-08T21:25:23Z
url: https://github.com/astral-sh/ruff/issues/360
synced_at: 2026-01-07T13:12:14-06:00
---

# False positive for W292

---

_Issue opened by @harupy on 2022-10-08 03:05_

### How to reproduce:

```
cargo run resources/test/fixtures/C402.py
```

### Expected:

W292 is not reported. `resources/test/fixtures/C402.py` does end with a newline.

### Actual:

```
resources/test/fixtures/C402.py:1:35: W292 No newline at end of file
```

---

_Comment by @harupy on 2022-10-08 06:48_

Related lines:

https://github.com/charliermarsh/ruff/blob/d9edec0ac9dabdd960002e5cf6b090e106f3d4ce/src/check_lines.rs#L43

https://github.com/charliermarsh/ruff/blob/d9edec0ac9dabdd960002e5cf6b090e106f3d4ce/src/check_lines.rs#L119

https://doc.rust-lang.org/std/string/struct.String.html#method.lines says:

> The final line ending is optional. A string that ends with a final line ending will return the same lines as an otherwise identical string without a final line ending.


---

_Comment by @harupy on 2022-10-08 06:50_

Can we just check whether `contents` ends with `\n` as follows?

```diff
diff --git a/src/check_lines.rs b/src/check_lines.rs
index f3b30be..8330cc8 100644
--- a/src/check_lines.rs
+++ b/src/check_lines.rs
@@ -117,7 +117,7 @@ pub fn check_lines(
     if settings.enabled.contains(&CheckCode::W292) {
         // If the file terminates with a newline, the last line should be an empty string slice.
         if let Some(line) = lines.last() {
-            if !line.is_empty() {
+            if !contents.ends_with("\n") {
                 let lineno = lines.len() - 1;
                 let noqa_lineno = noqa_line_for
                     .get(lineno)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-08 21:15_

---

_Referenced in [astral-sh/ruff#365](../../astral-sh/ruff/pulls/365.md) on 2022-10-08 21:24_

---

_Closed by @charliermarsh on 2022-10-08 21:25_

---
