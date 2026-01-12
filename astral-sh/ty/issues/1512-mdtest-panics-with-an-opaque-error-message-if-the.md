```yaml
number: 1512
title: "Mdtest panics with an opaque error message if the last line of an embedded Python file is a `# revealed` assertion"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
  - testing
assignees: []
created_at: 2025-11-10T10:30:51Z
updated_at: 2025-11-16T08:34:55Z
url: https://github.com/astral-sh/ty/issues/1512
synced_at: 2026-01-12T15:54:25Z
```

# Mdtest panics with an opaque error message if the last line of an embedded Python file is a `# revealed` assertion

---

_@AlexWaygood_

### Summary

To reproduce, apply this edit to the Ruff `main` branch:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/async.md b/crates/ty_python_semantic/resources/mdtest/async.md
index 0d57f4d8f8..e1c08c8b28 100644
--- a/crates/ty_python_semantic/resources/mdtest/async.md
+++ b/crates/ty_python_semantic/resources/mdtest/async.md
@@ -10,6 +10,8 @@ async def main():
     result = await retrieve()
 
     reveal_type(result)  # revealed: int
+
+# revealed: int
 ```
````

And then run `cargo test -p ty_python_semantic --test=mdtest`. The output is this:

```
failures:

---- mdtest__async stdout ----

async.md - `async` / `await` - Generic `async` func… (a4b8ea993d302151)


thread 'mdtest__async' (27624398) panicked at crates/ty_test/src/parser.rs:289:47:
Relative line number out of bounds
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    mdtest__async

test result: FAILED. 281 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 8.31s
```

The `# revealed: int` assertion is obviously invalid, because there is no line of Python below it containing a `reveal_type` call. But it would be better if we reported this as an mdtest diagnostic with a clear error message, rather than panicking with an opaque error message. That's the approach we take in other cases -- e.g. if I apply this diff to `main`:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/async.md b/crates/ty_python_semantic/resources/mdtest/async.md
index 0d57f4d8f8..c53b8e07ec 100644
--- a/crates/ty_python_semantic/resources/mdtest/async.md
+++ b/crates/ty_python_semantic/resources/mdtest/async.md
@@ -9,7 +9,7 @@ async def retrieve() -> int:
 async def main():
     result = await retrieve()
 
-    reveal_type(result)  # revealed: int
+    reveal_type(result)  # revealed:
 ```
 
 ## Generic `async` functions
````

then mdtest reports:

````
────────────────────────────────────────────────────────────────────────── Test for async.md failed ──────────────────────────────────────────────────────────────────────────

async.md - `async` / `await` - Basic (9c6054d8af10676d)

  crates/ty_python_semantic/resources/mdtest/async.md:12 invalid assertion: Must specify which type should be revealed
  crates/ty_python_semantic/resources/mdtest/async.md:12 used built-in `reveal_type`: add a `# revealed` assertion on this line (original diagnostic: 5 [undefined-reveal] "`reveal_type` used without importing it")
  crates/ty_python_semantic/resources/mdtest/async.md:12 unexpected error: 17 [revealed-type] "Revealed type: `int`"
```
````

which is a clear error message that identifies the cause of the failing assertion

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-11-10 10:30_

---

_Label `help wanted` added by @AlexWaygood on 2025-11-10 10:30_

---

_Label `testing` added by @AlexWaygood on 2025-11-10 10:30_

---

_Comment by @Hugo-Polloli on 2025-11-12 23:29_

Hi ! I tried something in the linked PR.
This is my first contribution to ty, I tried to adhere to the guidelines and contributing guide as much as I could but can't promise stellar work, thanks for tagging the PR as I was not sure about them

---

_Closed by @MichaReiser on 2025-11-16 08:34_

---
