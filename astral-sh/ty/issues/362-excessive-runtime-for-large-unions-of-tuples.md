```yaml
number: 362
title: Excessive runtime for large unions of tuples
type: issue
state: closed
author: nijel
labels:
  - bug
  - hang
assignees: []
created_at: 2025-05-13T17:48:25Z
updated_at: 2025-08-15T15:16:12Z
url: https://github.com/astral-sh/ty/issues/362
synced_at: 2026-01-10T02:06:24Z
```

# Excessive runtime for large unions of tuples

---

_Issue opened by @nijel on 2025-05-13 17:48_

### Summary

I wanted to give `ty` a try, but it doesn't seem to complete at all on our repo.

I tried to reduce the scope, and it seems to hang on a single file:

```
uvx ty check weblate/settings_docker.py
```

The file in question is https://github.com/WeblateOrg/weblate/blob/main/weblate/settings_docker.py

Maybe I'm just too impatient, but I waited several minutes without any output.

### Version

0.0.1

---

_Label `bug` added by @sharkdp on 2025-05-13 18:01_

---

_Comment by @sharkdp on 2025-05-13 18:04_

Thank you for reporting this!

From looking at the logs (`-vvv` in debug mode), it looks like we have problems with the large tuple types that are being constructed in that file. So it might be an instance of #71 (Edit: well, probably not).

---

_Label `hang` added by @MichaReiser on 2025-05-13 18:09_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-13 18:48_

---

_Renamed from "hangs on some files" to "Excessive runtime for large unions of tuples" by @sharkdp on 2025-05-13 18:52_

---

_Comment by @sharkdp on 2025-05-13 19:02_

The code in question here essentially does something like the following (see the conditional `AUTHENTICATION_BACKENDS += …` assignments in the original file):

```py
t = ()
if <some check>:
    t += (1,)
if <some check>:
    t += (2,)
if <some check>:
    t += (3,)
if <some check>:
    t += (4,)
# …
if <some check>:
    t += (N,)
```

Since every check can be either true or false, this (tries to) build up a union-of-tuples type with 2^N elements. In the original file, N = 24, so that's ~16 million union elements.

`pyright` handles this by falling back to `tuple[()] | tuple[Literal[1]] | tuple[int, ...] `. Not sure what the exact heuristic is, but at some point it adds `tuple[U, ...]` members (potential several of them), where `U` is a combination of (literal-widened) types of respective tuple elements.

---

_Comment by @nijel on 2025-05-14 06:38_

Is there really heuristic needed? In this particular case, the variable is type annotated since the beginning:

```py
AUTHENTICATION_BACKENDS: tuple[str, ...] = ()
```

---

_Comment by @sharkdp on 2025-05-14 06:46_

We generally keep track of both: the declared type (if available) and the inferred type. This allows us to check each of the annotated assignments (is the inferred type assignable to the declared type?). And in general, this allows the inferred type to be "narrower" than the declared type. This can be useful if you have something like:
```py
def get_int() -> int:
    return -17


x: int | None = get_int()

reveal_type(x)  # int

if x < 0:  # declared type is `int | None`, but inferred type is `int`, so this is fine.
    x = None

reveal_type(x)  # int | None
```

So that declared type of `tuple[str, ...]` doesn't help us here. And even if it would, we'd still need to make sure that we can handle the same situation without a declared type.

---

_Unassigned @sharkdp by @sharkdp on 2025-05-14 12:04_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:24_

---

_Comment by @carljm on 2025-08-15 15:16_

We removed precise inference for tuple concatenation, so the issue reported here (in this form) should be fixed now.

---

_Closed by @carljm on 2025-08-15 15:16_

---
