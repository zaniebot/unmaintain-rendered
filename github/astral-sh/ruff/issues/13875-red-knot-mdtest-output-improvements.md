---
number: 13875
title: "[red-knot] mdtest output improvements"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - testing
  - ty
assignees: []
created_at: 2024-10-22T07:45:28Z
updated_at: 2024-12-12T12:40:18Z
url: https://github.com/astral-sh/ruff/issues/13875
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] mdtest output improvements

---

_Issue opened by @MichaReiser on 2024-10-22 07:45_

Some ideas on how we could improve the mdtest output:

## Remove redundant test name

The test name, e.g. `imports/errors` is now rendered in at least 3 places:

```
failures:

---- mdtest__import_errors stdout ----

/import/errors.md - Unresolved Imports - Unresolved import from statement

    /import/errors.md:16 unmatched assertion: revealed: Unknown2
    /import/errors.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

--------------------------------------------------

thread 'mdtest__import_errors' panicked at crates/red_knot_test/src/lib.rs:61:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

1. `mdtest_import_errors stdout` (`cargo test`)
2. The error header: `/import/errors.md - Unresolved Imports - Unresolved import from statement` (mdtest)
3. The cargo test error message: `thread 'mdtest__import_errors' panicked at crates/red_knot_test/src/lib.rs:61:5:`

It's even worse when using `nextest` 


```
--- STDOUT:              red_knot_python_semantic::mdtest mdtest__import_errors ---

running 1 test

/import/errors.md - Unresolved Imports - Unresolved import from statement

    /import/errors.md:16 unmatched assertion: revealed: Unknown2
    /import/errors.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

--------------------------------------------------

test mdtest__import_errors ... FAILED

failures:

failures:
    mdtest__import_errors

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 67 filtered out; finished in 0.16s


--- STDERR:              red_knot_python_semantic::mdtest mdtest__import_errors ---
thread 'mdtest__import_errors' panicked at crates/red_knot_test/src/lib.rs:61:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

I suggest that we change the `mdtest` header to just show the test-name, excluding the markdown file:

```

failures:

---- mdtest__import_errors stdout ----

Unresolved Imports - Unresolved import from statement

    /import/errors.md:16 unmatched assertion: revealed: Unknown2
    /import/errors.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

--------------------------------------------------

thread 'mdtest__import_errors' panicked at crates/red_knot_test/src/lib.rs:61:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

## File paths not clickable

The `errors.md:16` file paths aren't clickable for me because the paths' aren't relative to the workspace root. We should make the paths relative to the workspace root (and render them as relative and not absolute paths)

## Reduce indentation

I find the indentation of the inner errors distracting. I don't think we need it considering that we use blank lines and a bold title to group files. 


```

failures:

---- mdtest__import_errors stdout ----

Unresolved Imports - Unresolved import from statement

/import/errors.md:16 unmatched assertion: revealed: Unknown2
/import/errors.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

Unresolved Imports2 - Unresolved import from statement2

/import/errors.md:16 unmatched assertion: revealed: Unknown2
/import/errors.md:16 unexpected error: 1 [revealed-type] "Revealed type is `Unknown`"

--------------------------------------------------

thread 'mdtest__import_errors' panicked at crates/red_knot_test/src/lib.rs:61:5:
Some tests failed.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `help wanted` added by @MichaReiser on 2024-10-22 07:45_

---

_Label `red-knot` added by @MichaReiser on 2024-10-22 07:45_

---

_Comment by @AlexWaygood on 2024-10-23 19:10_

> Remove redundant test name

Definitely.

> File paths not clickable

Huh, they're clickable for me in VSCode. But I definitely don't object to this change.

> Reduce indentation
> 
> I find the indentation of the inner errors distracting. I don't think we need it considering that we use blank lines and a bold title to group files.

One thing about this is that I do think the indentation makes the output more readable when long errors "spill over" onto multiple lines. Here's one output as it is now:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/955d4106-d584-41e2-a7fc-77eb3064138c)

</details>

And here it is without the indentation:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/19dceeb6-43c4-4cfe-891b-43103f11ae27)

</details>

<details>

<summary>Diff to produce the errors used for screenshots</summary>

````diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md b/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
index f7cf500d0..fec12a7b8 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md
@@ -92,15 +92,15 @@ def could_raise_returns_str() -> str:
 x = 1
 
 try:
-    reveal_type(x)  # revealed: Literal[1]
+    reveal_type(x)
     x = could_raise_returns_str()
-    reveal_type(x)  # revealed: str
+    reveal_type(x)  # revealed: int
 except TypeError:
     reveal_type(x)  # revealed: Literal[1] | str
     x = 2
-    reveal_type(x)  # revealed: Literal[2]
+    reveal_type(x)
 
-reveal_type(x)  # revealed: str | Literal[2]
+reveal_type(x)  # revealed: str
 ```
 
 ## Multiple `except` branches
````

</details>

I agree in most cases the indentation feels unnecessary though. Ideally I feel like I'd like the _second_ line of the "spillover" for very long error messages to be indented, rather than the first line. Not sure how tricky that would be.

---

_Label `testing` added by @AlexWaygood on 2024-10-23 19:12_

---

_Comment by @MichaReiser on 2024-10-24 06:07_

> Ideally I feel like I'd like the second line of the "spillover" for very long error messages to be indented, rather than the first line. Not sure how tricky that would be.

It depends. If we would want to indent after every newline, then this `Formatter` adapter might be useful
[Than](https://github.com/astral-sh/ruff/blob/e7b49694a795e3347ffc6f499245dfcbbb4b28ed/crates/ruff_linter/src/message/grouped.rs#L173-L202)

But that's not what we need here because there's no newline. The line wraps because your terminal doesn't have enough space to render the entire line. A trivial solution could be chunk messages into 60 char long parts and render them by part. I think the long-term solution is that we build a proper diagnostic system that supports showing rich content. It helps reducing the need for long messages.

---

_Referenced in [astral-sh/ruff#14213](../../astral-sh/ruff/pulls/14213.md) on 2024-11-08 22:32_

---

_Closed by @MichaReiser on 2024-12-12 12:40_

---
