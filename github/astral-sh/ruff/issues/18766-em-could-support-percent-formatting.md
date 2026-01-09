---
number: 18766
title: EM could support percent formatting
type: issue
state: open
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-06-18T18:48:11Z
updated_at: 2025-06-18T20:46:25Z
url: https://github.com/astral-sh/ruff/issues/18766
synced_at: 2026-01-07T13:12:16-06:00
---

# EM could support percent formatting

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 18:48_

### Summary

Currently `EM` only has rules checking for f-strings and `.format`. It could also support percent formatting, as in
```py
raise RuntimeError("timed out waiting for file: %s" % path)
```
But this is currently missed [playground](https://play.ruff.rs/35bb2b34-3974-4562-83db-96772e94820e)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
raise RuntimeError("timed out waiting for file: %s" % path)
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select EM
```
```
All checks passed!
```
Currently this can be fixed if `UP031` is also enabled, since it converts the percent formatting to a fixable `.format` [playground](https://play.ruff.rs/fb46314a-2ca8-4497-822c-918d27770cd1)
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select EM --select UP031 --unsafe-fixes --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1 +1,2 @@
-raise RuntimeError("timed out waiting for file: %s" % path)
+msg = "timed out waiting for file: {}".format(path)
+raise RuntimeError(msg)

Would fix 2 errors.
```
However, this does not work if the string is a byte string, since `UP031` does not/cannot support byte strings [playground](https://play.ruff.rs/2952b1df-2101-495a-84de-d375c66204d4)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
raise RuntimeError(b"timed out waiting for file: %s" % path)
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select EM --select UP031 --unsafe-fixes
```
```
All checks passed!
```
Which is where `EM` supporting percent formatting would be useful. This is also not unheard of, as several of the results from a [github search](https://github.com/search?q=language%3APython+raise+RuntimeError%28b%22&type=code) for `raise RuntimeError(b"` use byte string percent formatting.

---

_Comment by @MichaReiser on 2025-06-18 20:37_

I think this was raised before (on my phone where searching issues is a bit painful). The reasoning why we don't support it is because percent formatting comes with a lot of edge cases and those rules are fairly complex. We don't want to repeat the complexity and it shouldn't matter for most users if they have both fixes enabled. 

I believe you raised another issue to add support for Byte strings, so that it would be covered too

---

_Comment by @MeGaGiGaGon on 2025-06-18 20:43_

#18765 is for `EM101` specifically, while this would be for a new rule. This issue is mostly targeting the edge case where the message is a byte string that uses percent formatting, since that can't be turned into a `.format` by `UP031`, so it currently can't be handled by `EM` despite being a duplicate message. I also can't find an issue where someone requested this before, I may be missing it though.

---
