```yaml
number: 18787
title: "[Perflint] PERF401/PERF403 fix can duplicate comments"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-19T07:38:58Z
updated_at: 2025-06-19T22:35:07Z
url: https://github.com/astral-sh/ruff/issues/18787
synced_at: 2026-01-10T11:09:58Z
```

# [Perflint] PERF401/PERF403 fix can duplicate comments

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 07:38_

### Summary

With preview enabled, the fix for `PERF401` can duplicate comments if they are inside an empty tuple inside the `if` test. [playground](https://play.ruff.rs/7a4d8896-1d7a-45c5-96c9-5f92b73709ff)
```
PS D:\python_projects> Get-Content issue.py
```
```py
original = list(range(10000))
filtered = []
for i in original:
    if (
        # comment
    ):
        filtered.append(i)
```
```
PS D:\python_projects> uvx ruff check issue.py --select PERF --preview
```
```snap
issue.py:7:9: PERF401 Use a list comprehension to create a transformed list
  |
5 |         # comment
6 |     ):
7 |         filtered.append(i)
  |         ^^^^^^^^^^^^^^^^^^ PERF401
  |
  = help: Replace for loop with list comprehension

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
```
PS D:\python_projects> uvx ruff check issue.py --select PERF --preview --unsafe-fixes --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,7 +1,5 @@
 original = list(range(10000))
-filtered = []
-for i in original:
-    if (
+# comment
+filtered = [i for i in original if (
         # comment
-    ):
-        filtered.append(i)
+    )]

Would fix 1 error.
```

This also applies to `PERF403` [playground](https://play.ruff.rs/7f71ad67-7481-41a5-8b69-7da107f499b2)

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Renamed from "[Perflint] PERF401 fix can duplicate comments" to "[Perflint] PERF401/PERF403 fix can duplicate comments" by @MeGaGiGaGon on 2025-06-19 07:42_

---

_Label `bug` added by @MichaReiser on 2025-06-19 09:02_

---

_Label `help wanted` added by @MichaReiser on 2025-06-19 09:02_

---

_Comment by @LaBatata101 on 2025-06-19 22:06_

What would be the correct fix for this, delete the comment inside the `if` or the comment outside?

---

_Comment by @MeGaGiGaGon on 2025-06-19 22:35_

I'm not sure if this is the best answer, but I think the current implementation is over-zealous at formatting, which is why so many comments need to be moved. If it kept the original formatting of the code, there are a good amount of places where comments don't need to be moved.
```py
original = list(range(10000))
filtered = []
for (  # doesn't need to be moved 1
    i  # doesn't need to be moved 2
) in (  # doesn't need to be moved 3
    original  # doesn't need to be moved 4
):  # needs to be moved 5
    # needs to be moved 6
    if (  # doesn't need to be moved 7
        1,  # doesn't need to be moved 8
    ):  # needs to be moved 9
        # needs to be moved 10
        filtered.append(  # doesn't need to be moved 11
            i  # doesn't need to be moved 12
        )  # doesn't need to be moved 13
```
The playground currently does [this](https://play.ruff.rs/9fc6d544-73ec-4edf-b0b8-1a1ad6c96f1d):
```py
original = list(range(10000))
# doesn't need to be moved 1
# doesn't need to be moved 2
# doesn't need to be moved 3
# doesn't need to be moved 4
# needs to be moved 5
# needs to be moved 6
# doesn't need to be moved 7
# doesn't need to be moved 8
# needs to be moved 9
# needs to be moved 10
# doesn't need to be moved 11
# doesn't need to be moved 12
filtered = [i for i in original if (  # doesn't need to be moved 7
        1,  # doesn't need to be moved 8
    )]  # doesn't need to be moved 13
```
But all those could remain unmoved since they are within an expression that needs to have parenthesis to be multiline anyways
```py
# needs to be moved 5
# needs to be moved 6
# needs to be moved 9
# needs to be moved 10
filtered = [(  # doesn't need to be moved 11
    i  # doesn't need to be moved 12
) for (  # doesn't need to be moved 1
    i  # doesn't need to be moved 2
) in (  # doesn't need to be moved 3
    original  # doesn't need to be moved 4
) if (  # doesn't need to be moved 7
        1,  # doesn't need to be moved 8
)]  # doesn't need to be moved 13
```

Another option would be to always move all comments for consistency.

Not sure which one though, or if there are more besides these two.

---
