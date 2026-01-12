```yaml
number: 14213
title: "[red-knot] Improve mdtest output"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/better-mdtest-output
created_at: 2024-11-08T22:31:59Z
updated_at: 2024-11-11T11:03:43Z
url: https://github.com/astral-sh/ruff/pull/14213
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Improve mdtest output

---

_@AlexWaygood_

## Summary

Helps with #13875. Changes made:
- Paths given for the Markdown files are relative to the workspace root rather than the crate root. This enables you to copy-and-paste them straight from the terminal, and makes them clickable in more editors.
- Use shorter titles for the bold/underlined section titles: just the filename for the first part of the title, rather than the path of the Markdown file relative to `crates/red_knot_python_semantic/resources/mdtest`. I didn't get rid of these entirely, because I believe it's possible to have a Markdown test file that doesn't have any `#` headings in it, which I believe would result in an empty title if we got rid of the path from the title entirely.
- Reduce the indentation of the line-by-line error reports from 4 spaces to 2. I'm not wild about getting rid of these entirely for the reasons I gave in https://github.com/astral-sh/ruff/issues/13875#issuecomment-2433225174, but I think it makes sense to reduce them a bit.

Previous output:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/ed5fab80-1800-4ad5-a0eb-89d13becaed0)

</details>

New output:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/39f46474-7dc1-418a-8f96-d28db1e33d7b)

</details>

Diff to achieve these demo test failures:

<details>

````diff
--- a/crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md
@@ -1,5 +1,7 @@
 # Identity tests
 
+## Foo
+
 ```py
 class A: ...
 
@@ -14,7 +16,7 @@ n2 = None
 
 o = get_object()
 
-reveal_type(a1 is a1)  # revealed: bool
+reveal_type(a1 is a1)  # revealed: str
 reveal_type(a1 is a2)  # revealed: bool
 
 reveal_type(n1 is n1)  # revealed: Literal[True]
@@ -38,3 +40,9 @@ reveal_type(n1 is not a1)  # revealed: Literal[True]
 reveal_type(a1 is not o)  # revealed: bool
 reveal_type(n1 is not o)  # revealed: bool
 ```
+
+## Bar
+
+```py
+x
+```
````

</details>

## Test Plan

`cargo test`


---

_Review requested from @carljm by @AlexWaygood on 2024-11-08 22:32_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-08 22:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-08 22:32_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-08 22:32_

---

_Comment by @github-actions[bot] on 2024-11-08 22:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Converted to draft by @AlexWaygood on 2024-11-08 22:49_

---

_Marked ready for review by @AlexWaygood on 2024-11-08 23:50_

---

_@carljm approved on 2024-11-09 18:18_

I don't love how long the file path prefix for every error message is now, but if this is what's required to make them clickable in more editors, I guess it's worth it? I don't use the clickability myself. Don't really see a better solution, other than moving the mdtest files to some shorter path relative to workspace root.

It's probably possible to only include the file name in the test name if there is no header? Though it also doesn't bother me to always include it.

On the whole I don't have super strong preferences here so you could wait for a review from Micha, who probably has more opinions. But this looks fine to me.

---

_Comment by @AlexWaygood on 2024-11-09 19:15_

> I don't love how long the file path prefix for every error message is now, but if this is what's required to make them clickable in more editors, I guess it's worth it? I don't use the clickability myself.

I don't use the clickability inside my IDE much either, but what I _do_ do is run tests from a terminal window outside of my IDE. I'm finding it consistently annoying right now that I can't copy and paste the path directly from the mdtest output so that I can instantly open the Markdown file in question in a new text-editor window. So while I agree that some output lines are now very long with this PR, this would be quite a big quality-of-life improvement for me.

> It's probably possible to only include the file name in the test name if there is no header?

I'm sure it is possible. I looked into it quickly, and it seemed more fiddly than I expected, though, and it didn't seem to me to be important enough for me to spend a lot of time on it. This was easy to do, and seemed like an improvement on the status quo.

---

_Comment by @sharkdp on 2024-11-11 08:50_

> I don't love how long the file path prefix for every error message is now, but if this is what's required to make them clickable in more editors, I guess it's worth it? I don't use the clickability myself. Don't really see a better solution, other than moving the mdtest files to some shorter path relative to workspace root.

We could use [OSC 8 hyperlinks](https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda) to potentially have the best of both worlds? We can keep the file path in the output short and provide the full path (including the line number) as a hyperlink.

The problem is that there is no standardized format for *"open file F at line L in [your editor of choice]"*. Some applications solve this by making the hyperlink format customizable. If this doesn't sound too over-engineered, we could maybe provide 2-3 common formats (and a fallback to simple `file://hostname/path/to/file` links if nothing else was specified) and let every developer configure it (env. variable?!).

Example for VS Code (adapt the `/path/to/ruff/` and paste into a terminal to try. `vscode://file/` is the prefix that needs to be kept intact):
```
printf '\033]8;;vscode://file/path/to/ruff/crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md:19\033\\comparison/identity_tests.md:19\033]8;;\033\\\n'
```

Example with basic `file://` link; no support for line numbers; hostname is mandatory. 
```
printf '\033]8;;file://localhost/path/to/ruff/crates/red_knot_python_semantic/resources/mdtest/comparison/identity_tests.md\033\\comparison/identity_tests.md:19\033]8;;\033\\\n'
```

---

_Comment by @MichaReiser on 2024-11-11 09:45_

> On the whole I don't have super strong preferences here so you could wait for a review from Micha, who probably has more opinions. But this looks fine to me.

I've been pretty annoyed by the links not being clickable for the few tests I wrote. That's why I find this a huge improvement. 

IMO, the ultimate solution is to use a multiline representation to render the diagnostics, so that the file names no longer stand out that much

---

_@MichaReiser approved on 2024-11-11 09:48_

> I didn't get rid of these entirely, because I believe it's possible to have a Markdown test file that doesn't have any # headings in it, which I believe would result in an empty title if we got rid of the path from the title entirely.

Could we only show the filename if the title is empty or change the test framework so that it defaults to the filename if the test has no explicit title?

> Reduce the indentation of the line-by-line error reports from 4 spaces to 2. I'm not wild about getting rid of these entirely for the reasons I gave in

This might be a problem for accessibility reasons. I'm still leaning towards removing them entirely but I can also do this when we rework our diagnostics ;)

Thanks

---

_Comment by @AlexWaygood on 2024-11-11 11:03_

> Could we only show the filename if the title is empty or change the test framework so that it defaults to the filename if the test has no explicit title?

I already answered this in https://github.com/astral-sh/ruff/pull/14213#issuecomment-2466416419 ;-) I'm sure it's possible, but didn't want to spend too much time on it

> We could use [OSC 8 hyperlinks](https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda) to potentially have the best of both worlds? We can keep the file path in the output short and provide the full path (including the line number) as a hyperlink.

This could be great!! I'll merge this as-is for now, though, as I think we're aligned that, even though the output still isn't ideal, this is probably a strict improvement on the status quo :-) This feels like it could be pursued as a followup

---

_Merged by @AlexWaygood on 2024-11-11 11:03_

---

_Closed by @AlexWaygood on 2024-11-11 11:03_

---

_Branch deleted on 2024-11-11 11:03_

---
