```yaml
number: 18915
title: "py-fuzzer: allow relative executable paths"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
assignees: []
merged: true
base: main
head: david/fuzzer-relative-paths
created_at: 2025-06-24T12:32:11Z
updated_at: 2025-06-24T13:16:22Z
url: https://github.com/astral-sh/ruff/pull/18915
synced_at: 2026-01-12T15:56:27Z
```

# py-fuzzer: allow relative executable paths

---

_@sharkdp_

## Summary

I tried running `py-fuzzer` using executables in the current working directory, but that failed with:
```
▶ uvx --from ./python/py-fuzzer --reinstall fuzz --test-executable ./ty_feature --bin=ty --baseline-executable ./ty_main --only-new-bugs 0-500
Usage: fuzz [-h] [--only-new-bugs] [--quiet] [--test-executable TEST_EXECUTABLE] [--baseline-executable BASELINE_EXECUTABLE] --bin {ruff,ty} seeds [seeds ...]
fuzz: error: Bad argument passed to `--baseline-executable`: no such file or executable PosixPath('ty_main')
 "Bad argument passed to `--baseline-executable`: no such file or executable PosixPath('ty_main')"
```

Using `.absolute()` on the `Path` fixes this.


## Test Plan

Successful `py-fuzzer` run with the invocation above.

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-24 12:32_

---

_Label `internal` added by @sharkdp on 2025-06-24 12:32_

---

_Comment by @AlexWaygood on 2025-06-24 12:41_

Or we could call `resolve()` eagerly when parsing the CLI arguments?

```diff
diff --git a/python/py-fuzzer/fuzz.py b/python/py-fuzzer/fuzz.py
index 226cab31fa..5359e28273 100644
--- a/python/py-fuzzer/fuzz.py
+++ b/python/py-fuzzer/fuzz.py
@@ -54,9 +54,7 @@ def ty_contains_bug(code: str, *, ty_executable: Path) -> bool:
         input_file = Path(tempdir, "input.py")
         input_file.write_text(code)
         completed_process = subprocess.run(
-            [ty_executable.resolve(), "check", input_file],
-            capture_output=True,
-            text=True,
+            [ty_executable, "check", input_file], capture_output=True, text=True
         )
     return completed_process.returncode not in {0, 1, 2}
 
@@ -308,6 +306,10 @@ class ResolvedCliArgs:
     quiet: bool
 
 
+def resolved_path(p: str) -> Path:
+    return Path(p).resolve()
+
+
 def parse_args() -> ResolvedCliArgs:
     """Parse command-line arguments"""
     parser = argparse.ArgumentParser(
@@ -339,7 +341,7 @@ def parse_args() -> ResolvedCliArgs:
             "Executable to test. "
             "Defaults to a fresh build of the currently checked-out branch."
         ),
-        type=Path,
+        type=resolved_path,
     )
     parser.add_argument(
         "--baseline-executable",
@@ -348,7 +350,7 @@ def parse_args() -> ResolvedCliArgs:
             "Defaults to whatever version is installed "
             "in the Python environment."
         ),
-        type=Path,
+        type=resolved_path,
     )
     parser.add_argument(
         "--bin",
@@ -369,9 +371,7 @@ def parse_args() -> ResolvedCliArgs:
             )
         try:
             subprocess.run(
-                [args.baseline_executable.resolve(), "--version"],
-                check=True,
-                capture_output=True,
+                [args.baseline_executable, "--version"], check=True, capture_output=True
             )
         except FileNotFoundError:
             parser.error(
```

---

_@AlexWaygood approved on 2025-06-24 12:41_

---

_Comment by @github-actions[bot] on 2025-06-24 12:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-06-24 12:42_

Or use [`absolute`](https://docs.python.org/3/library/pathlib.html#pathlib.Path.absolute) because we don't need to resolve symlinks ;)

---

_Merged by @sharkdp on 2025-06-24 13:16_

---

_Closed by @sharkdp on 2025-06-24 13:16_

---

_Branch deleted on 2025-06-24 13:16_

---
