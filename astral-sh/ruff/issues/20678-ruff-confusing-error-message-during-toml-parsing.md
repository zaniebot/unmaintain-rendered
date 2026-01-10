---
number: 20678
title: "[`ruff`] Confusing error message during TOML parsing"
type: issue
state: closed
author: chirizxc
labels: []
assignees: []
created_at: 2025-10-02T09:26:24Z
updated_at: 2025-10-02T14:36:12Z
url: https://github.com/astral-sh/ruff/issues/20678
synced_at: 2026-01-10T01:23:01Z
---

# [`ruff`] Confusing error message during TOML parsing

---

_Issue opened by @chirizxc on 2025-10-02 09:26_

### Summary

`ruff.toml`:
```toml
target-version = "py312"

line-length = 90

[tool.lint.isort]  # <=== error here
force-wrap-aliases = true
combine-as-imports = true
no-lines-before = ["local-folder"]
```
```
❯ ruff check
ruff failed                                                                                                                                           
  Cause: Failed to load configuration `C:\Users\chiri\Desktop\ttt\ruff.toml`
  Cause: Failed to parse C:\Users\chiri\Desktop\ttt\ruff.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | target-version = "py312"
  | ^
unknown field `tool`
```
Arrow seems to be pointing in the wrong place

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)    

---

_Comment by @chirizxc on 2025-10-02 09:50_

I haven't checked all the other cases or run any tests, but something like this should work
```
❯ C:\Users\chiri\Desktop\ruff\target\debug\ruff.exe check .
ruff failed
  Cause: Failed to load configuration `C:\Users\chiri\Desktop\ttt\ruff.toml`
  Cause: TOML parse error at line 5, column 1
  |
5 | [tool.lint.isort]
  | ^
unknown field `tool`
```

```diff
❯ git diff 
diff --git a/crates/ruff_workspace/src/pyproject.rs b/crates/ruff_workspace/src/pyproject.rs                                                          
index 0f2251ee1..e104d3c44 100644
--- a/crates/ruff_workspace/src/pyproject.rs
+++ b/crates/ruff_workspace/src/pyproject.rs
@@ -45,7 +45,38 @@ fn parse_ruff_toml<P: AsRef<Path>>(path: P) -> Result<Options> {
     let path = path.as_ref();
     let contents = std::fs::read_to_string(path)
         .with_context(|| format!("Failed to read {}", path.display()))?;
-    toml::from_str(&contents).with_context(|| format!("Failed to parse {}", path.display()))
+
+    match toml::from_str::<Options>(&contents) {
+        Ok(options) => Ok(options),
+        Err(err) => {
+            let err_msg = err.to_string();
+
+            if let Some(field_start) = err_msg.find("unknown field `") {
+                if let Some(field_end) = err_msg[field_start + 15..].find('`') {
+                    let field_name = &err_msg[field_start + 15..field_start + 15 + field_end];
+
+                    let mut line_num = 1;
+                    for line in contents.lines() {
+                        let trimmed = line.trim_start();
+                        if trimmed.starts_with('[') && trimmed.contains(field_name) ||
+                            trimmed.starts_with(field_name) && trimmed.contains('=') {
+                            return Err(anyhow::anyhow!(
+                                "TOML parse error at line {}, column {}\n  |\n{} | {}\n  | ^\nunknown field `{}`",
+                                line_num,
+                                line.len() - trimmed.len() + 1,
+                                line_num,
+                                trimmed,
+                                field_name
+                            ));
+                        }
+                        line_num += 1;
+                    }
+                }
+            }
+
+            Err(err.into())
+        }
+    }
 }
```

---

_Renamed from "[`ruff`] Confusing error message" to "[`ruff`] Confusing error message when TOML parsing error" by @chirizxc on 2025-10-02 11:39_

---

_Renamed from "[`ruff`] Confusing error message when TOML parsing error" to "[`ruff`] Confusing error message during TOML parsing" by @chirizxc on 2025-10-02 11:39_

---

_Comment by @ntBre on 2025-10-02 14:36_

Thanks for the report and for looking into a patch! To me this is more of an issue in the `toml` package we use, and I found https://github.com/toml-rs/toml/issues/589, which seems related (although I'm not sure we're using `flatten` here).

Ah, and then linked from the `toml` issue, I found that this is a duplicate of one of our issues too: https://github.com/astral-sh/ruff/issues/12359.

I think we should continue waiting on the upstream instead of trying to reparse part of the TOML and the error message ourselves.

---

_Closed by @ntBre on 2025-10-02 14:36_

---
