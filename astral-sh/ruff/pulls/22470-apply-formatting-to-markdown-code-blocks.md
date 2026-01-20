```yaml
number: 22470
title: Apply formatting to markdown code blocks
type: pull_request
state: open
author: amyreese
labels: []
assignees: []
draft: true
base: main
head: amy/ruffen-docs
created_at: 2026-01-09T01:26:45Z
updated_at: 2026-01-20T21:48:41Z
url: https://github.com/astral-sh/ruff/pull/22470
synced_at: 2026-01-20T21:54:18Z
```

# Apply formatting to markdown code blocks

---

_@amyreese_

Adds initial support for formatting Python code blocks inside Markdown files.

- Adds `Markdown` source types/kinds
- Maps `.md` file extension to `Markdown` by default
- Uses simple regex adapted from blacken-docs to find and format fenced python code blocks
- Dedents contents before formatting, and reapplies indent from fenced <code>```py</code> header
- Selects `Python` vs `Stub` options based on language label on code block
- Silently skips formatting for any code block with syntax errors or that produce formatting errors.
- CLI tests formatting via both stdin and from filesystem
- Requires running with `--preview`, and otherwise warns user and skips formatting when given a markdown file
- Requires a user to `extend-include = ["**/*.md"]` if they want to format markdown files by default

Limitations:

- Raises `unimplemented!()` if run with a range of any sort
- Ignores unlabeled code blocks or implicit code blocks (no code fence)
- Doesn't yet support `~~~` fences, arbitrary fence lengths, or code blocks inside blockquotes

Issue #3792


---

_Comment by @astral-sh-bot[bot] on 2026-01-09 01:39_


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

_Renamed from "Ruffen docs prototype" to "WIP: Ruffen docs prototype" by @amyreese on 2026-01-09 16:35_

---

_Comment by @amyreese on 2026-01-13 02:21_

minimal working prototype using regex adapted from blacken-docs:

`````
amethyst@lunatone ~/workspace/ruff amy/ruffen-docs » cat ~/scratch/test.md
Hello, this is a *markdown* document.

This is a rust code block:

```rust
fn main() {
    for x in 0..10 {
        println!("x = {x}");
    }
}
```

This is a poorly formatted python code block:

```py
def foo(arg1,
           arg2):
    print( "hello world")



foo(1 , 2)
```

This is another python code block, also poorly formatted, but now in a list:

1. List item 1
2. List item 2

    ```python
    dataset = [1, 2, 3,
        4, 5, 6]
    if 1+2==3:
        print('yes')
    ```

And here's an unlabeled code block that happens to have valid python code — what do we do?

```
print("hello")
```

amethyst@lunatone ~/workspace/ruff amy/ruffen-docs » cargo run -p ruff -- format --no-cache --diff ~/scratch/test.md
   Compiling ruff v0.14.11 (/Users/amethyst/workspace/ruff/crates/ruff)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 1.66s
     Running `target/debug/ruff format --no-cache --diff /Users/amethyst/scratch/test.md`
--- /Users/amethyst/scratch/test.md
+++ /Users/amethyst/scratch/test.md
@@ -13,13 +13,11 @@
 This is a poorly formatted python code block:

 ```py
-def foo(arg1,
-           arg2):
-    print( "hello world")
+def foo(arg1, arg2):
+    print("hello world")


-
-foo(1 , 2)
+foo(1, 2)
 ```

 This is another python code block, also poorly formatted, but now in a list:
@@ -28,10 +26,9 @@
 2. List item 2

     ```python
-    dataset = [1, 2, 3,
-        4, 5, 6]
-    if 1+2==3:
-        print('yes')
+    dataset = [1, 2, 3, 4, 5, 6]
+    if 1 + 2 == 3:
+        print("yes")
     ```

 And here's an unlabeled code block that happens to have valid python code — what do we do?

1 file would be reformatted
> [1]

`````

---

_Comment by @amyreese on 2026-01-13 02:25_

Some notes on the regex adapted from blacken-docs:

- it does not support `~~~` delimited code blocks
- it does not support code blocks delimited by more than three backticks/tildes
- it does verify that indentation and the end delimiter matches the starting delimiter

Prototype also silently discards any formatting error, and those should either be warnings or get tracked somehow to re-raise them outside of the closure.

---

_Comment by @amyreese on 2026-01-13 02:25_

Also need to decide on if/how to gate this behind `--preview`

---

_Review requested from @MichaReiser by @amyreese on 2026-01-20 21:23_

---

_Review requested from @ntBre by @amyreese on 2026-01-20 21:23_

---

_Renamed from "WIP: Ruffen docs prototype" to "Apply formatting to markdown code blocks" by @amyreese on 2026-01-20 21:23_

---
