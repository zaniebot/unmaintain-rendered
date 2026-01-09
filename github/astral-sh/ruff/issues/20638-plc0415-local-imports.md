---
number: 20638
title: "PLC0415: local imports"
type: issue
state: closed
author: smurfix
labels:
  - rule
  - needs-info
assignees: []
created_at: 2025-09-29T21:08:12Z
updated_at: 2025-11-17T07:20:29Z
url: https://github.com/astral-sh/ruff/issues/20638
synced_at: 2026-01-07T13:12:16-06:00
---

# PLC0415: local imports

---

_Issue opened by @smurfix on 2025-09-29 21:08_

### Summary

This pattern is used often enough:

``` python
b64decode = None
...
def some_fn():
    global b64decode
    if b64decode is None:
        from base64 import b64decode  # noqa:PLC0415
```


---

_Renamed from "PLW0602, possibly PLC0415: false positive when importing" to "PLC0415: local imports" by @smurfix on 2025-09-29 21:10_

---

_Comment by @amyreese on 2025-09-30 16:23_

What is the issue here? Are you wanting this sort of pattern to be allowed, but it's currently flagged by the rule? 

I'm guessing this is an attempt at lazy imports, so moving it to global level is undesired. PLC0415 has support for ignoring specific imports if they are listed in `banned-module-level-imports`: https://docs.astral.sh/ruff/rules/import-outside-top-level/#see-also

---

_Label `rule` added by @amyreese on 2025-09-30 16:24_

---

_Label `needs-info` added by @MichaReiser on 2025-09-30 16:56_

---

_Closed by @MichaReiser on 2025-11-17 07:20_

---
