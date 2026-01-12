```yaml
number: 14001
title: "Formatter can insert mysterious extraneous parentheses in `with` statements"
type: issue
state: closed
author: lpulley
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-10-30T15:42:00Z
updated_at: 2025-03-11T08:35:18Z
url: https://github.com/astral-sh/ruff/issues/14001
synced_at: 2026-01-12T15:54:53Z
```

# Formatter can insert mysterious extraneous parentheses in `with` statements

---

_@lpulley_

My `ruff --version` is 0.7.1.

I've encountered a case where `ruff format` adds nesting parentheses for seemingly no reason:

```py
with (
    open(
        "some/path.txt",
        "rb",
    )
    if True
    else open("other/path.txt")
    # Bar
):
    pass
```

```
➜ ruff format --diff up034.py
--- up034.py
+++ up034.py
@@ -1,10 +1,12 @@
 with (
-    open(
-        "some/path.txt",
-        "rb",
+    (
+        open(
+            "some/path.txt",
+            "rb",
+        )
+        if True
+        else open("other/path.txt")
     )
-    if True
-    else open("other/path.txt")
     # Bar
 ):
     pass

1 file would be reformatted
```

i.e. it wants this:

```py
with (
    (
        open(
            "some/path.txt",
            "rb",
        )
        if True
        else open("other/path.txt")
    )
    # Bar
):
    pass
```

UP034 complains about this, for good reason.

If the `# Bar` comment is removed, the formatter makes no changes. More curiously, if I keep `# Bar` but add `as _`, the formatter makes no changes. (I also checked that Black does not do this.)

I've resolved the issue in the meantime by moving the `# Bar` comment to a different line, but this seems like a formatter bug.

---

_Label `bug` added by @MichaReiser on 2024-10-30 15:47_

---

_Label `formatter` added by @MichaReiser on 2024-10-30 15:47_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-10-30 15:47_

---

_Comment by @MichaReiser on 2024-10-30 17:43_

Thanks for reporting this. Agree, this looks silly but I'm a bit scared from fixing it... with item formatting is complicated.

---

_Closed by @MichaReiser on 2024-11-01 08:08_

---

_Comment by @lpulley on 2024-11-04 18:30_

Hmm... with Ruff 0.7.2, the example I gave above is still problematic; it still wants to add extra `(` `)` and fail UP034.

---

_Comment by @MichaReiser on 2024-11-04 19:52_

> Hmm... with Ruff 0.7.2, the example I gave above is still problematic; it still wants to add extra `(` `)` and fail UP034.

Yeah, we can't fix it before promoting the new style to stable. You would have to use `format.preview = true` for now

---

_Comment by @lpulley on 2024-11-04 19:55_

Aha; I missed that. Thanks for the clarification!!

-------- Original Message --------
On 11/4/24 13:52, Micha Reiser  wrote:

>> Hmm... with Ruff 0.7.2, the example I gave above is still problematic; it still wants to add extra ( ) and fail UP034.
>
> Yeah, we can't fix it before promoting the new style to stable. You would have to use format.preview = true for now
>
> —
> Reply to this email directly, [view it on GitHub](https://github.com/astral-sh/ruff/issues/14001#issuecomment-2455575739), or [unsubscribe](https://github.com/notifications/unsubscribe-auth/ABW4EY3TB6AAI6UTPRAK5FDZ67GBFAVCNFSM6AAAAABQ4NIQLOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDINJVGU3TKNZTHE).
> You are receiving this because you authored the thread.Message ID: ***@***.***>

---

_Comment by @MichaReiser on 2024-11-04 19:57_

You can subscribe to #13371 to get notified when the preview style gets stabilized.

---

_Comment by @lpulley on 2025-01-09 15:15_

I'm trying out 0.9.0 and still seeing this behavior. Did this make it into the 2025 style?

---

_Comment by @MichaReiser on 2025-01-09 15:23_

Hmmm, no. That's on me.

---

_Comment by @lpulley on 2025-01-09 15:26_

Not the end of the world; just wanted to make sure I wasn't missing anything obvious. Thanks.

---

_Comment by @MichaReiser on 2025-01-09 15:31_

Thanks for being so understanding and sorry that I missed that one last preview style

---

_Comment by @cj81499 on 2025-01-09 16:01_

Hey @MichaReiser, I'm one of @lpulley's coworkers. Will this make it into 0.9.1? Or will we be waiting till the 2026 style?

fwiw, we've been super happy with ruff and this will not change that :) Thanks for helping to build such an awesome piece of software!

---

_Comment by @MichaReiser on 2025-01-10 07:16_

Thanks for the kind words. 

We might want to pull it into 0.10, considering that changes should be super rare. I don't feel comfortable shipping it as part of a patch release because it would violate our versioning policy

---

_Reopened by @MichaReiser on 2025-01-10 07:16_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-01-10 07:16_

---

_Comment by @cj81499 on 2025-01-10 14:01_

Sounds good to me, thank you!

---

_Closed by @MichaReiser on 2025-03-11 08:34_

---

_Comment by @MichaReiser on 2025-03-11 08:35_

This fix is now merged into the release branch for 0.10

---
