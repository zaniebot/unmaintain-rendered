---
number: 21384
title: "Request: Adjust Rule \"superfluous-else-return (RET505)\""
type: issue
state: closed
author: asilvester-nw
labels:
  - question
assignees: []
created_at: 2025-11-11T15:53:27Z
updated_at: 2025-11-12T18:11:34Z
url: https://github.com/astral-sh/ruff/issues/21384
synced_at: 2026-01-07T13:12:16-06:00
---

# Request: Adjust Rule "superfluous-else-return (RET505)"

---

_Issue opened by @asilvester-nw on 2025-11-11 15:53_

### Summary

With the following code snippet, I get this (RET505) linter error on the first "elif". Based off the ruff documentation, this should only apply on an else block that had a return inside of it.

```
if request.mode == "search":
   return "search"
elif request.mode == "agent":
    return "agent"
elif request.mode == "analyst":
    return "analyst"
```

---

_Comment by @ntBre on 2025-11-11 16:17_

I think it makes sense for the rule to apply to both `else` and `elif`. This is also inline with the upstream linter:

```console
# uvx --with flake8-return flake8 - --select R505 <<EOF
def foo():
    if request.mode == "search":
       return "search"
    elif request.mode == "agent":
        return "agent"
    elif request.mode == "analyst":
        return "analyst"
EOF
stdin:2:5: R505 unnecessary elif after return statement.
```

---

_Label `question` added by @ntBre on 2025-11-11 16:17_

---

_Comment by @asilvester-nw on 2025-11-11 16:41_

Doing some more reading on this, I can see the argument for this. I was thinking about this from the manner of "only one of these conditions should trigger", so keeping them in the same if-elif-else code block follows that logic intuitively. 

Two questions:

1. Right now, the auto-fix just changes the first `elif` to and `if` but I believe it should be applied to both `elif`.
2. If an `else` statement is added, should we expect the rule to apply?

---

_Comment by @ntBre on 2025-11-11 16:45_

If you run Ruff in the CLI, both `elif` branches are fixed:

```diff
> ruff check --select RET505 --diff
--- try.py
+++ try.py
@@ -1,7 +1,7 @@
 def foo():
     if request.mode == "search":
        return "search"
-    elif request.mode == "agent":
+    if request.mode == "agent":
         return "agent"
-    elif request.mode == "analyst":
+    if request.mode == "analyst":
         return "analyst"

Would fix 2 errors.
```

And an additional else is also fixed:

```diff
 > ruff check --select RET505 --diff
--- try.py
+++ try.py
@@ -1,9 +1,8 @@
 def foo():
     if request.mode == "search":
        return "search"
-    elif request.mode == "agent":
+    if request.mode == "agent":
         return "agent"
-    elif request.mode == "analyst":
+    if request.mode == "analyst":
         return "analyst"
-    else:
-        return None
+    return None

Would fix 3 errors.
```

Is that what you had in mind?

If you like the if-elif sequence in this case, you can always use a `noqa` comment to suppress the rule locally :)

---

_Comment by @asilvester-nw on 2025-11-12 18:11_

Yes, this is what I had in mind. I did realize that I was unable to produce this exactly because I have try and except statements in the last two `elif` blocks but the `if` block only contained a return statement.  

---

_Closed by @asilvester-nw on 2025-11-12 18:11_

---
