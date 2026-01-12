```yaml
number: 18843
title: "[`refurb`] `FURB129` fix can delete comments"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-21T00:33:08Z
updated_at: 2025-06-22T07:51:38Z
url: https://github.com/astral-sh/ruff/issues/18843
synced_at: 2026-01-12T15:54:56Z
```

# [`refurb`] `FURB129` fix can delete comments

---

_@MeGaGiGaGon_

### Summary

The fix for [readlines-in-for (FURB129)](https://docs.astral.sh/ruff/rules/readlines-in-for/#readlines-in-for-furb129) can delete comments. It should be an unsafe fix in this case. [playground](https://play.ruff.rs/7bc42e09-a6ae-4151-85b0-0f03b908746f)
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
with open("file.txt") as fp:
    for line in (  # 1
        fp  # 2
        .  # 3
        readlines  # 4
        (  # 5
        )  # 6
    ):
        ...
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select FURB
```
```snap
issue.py:3:9: FURB129 [*] Instead of calling `readlines()`, iterate over file object directly
  |
1 |   with open("file.txt") as fp:
2 |       for line in (  # 1
3 | /         fp  # 2
4 | |         .  # 3
5 | |         readlines  # 4
6 | |         (  # 5
7 | |         )  # 6
  | |_________^ FURB129
8 |       ):
9 |           ...
  |
  = help: Remove `readlines()`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select FURB --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,9 +1,5 @@
 with open("file.txt") as fp:
     for line in (  # 1
-        fp  # 2
-        .  # 3
-        readlines  # 4
-        (  # 5
-        )  # 6
+        fp  # 6
     ):
         ...

Would fix 1 error.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @MichaReiser on 2025-06-21 15:38_

---

_Label `help wanted` added by @MichaReiser on 2025-06-21 15:38_

---

_Closed by @MichaReiser on 2025-06-22 07:51_

---
