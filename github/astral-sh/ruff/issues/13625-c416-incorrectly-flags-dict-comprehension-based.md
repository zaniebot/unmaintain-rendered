---
number: 13625
title: C416 incorrectly flags dict comprehension based on dict with tuple keys
type: issue
state: open
author: henribru
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-10-04T14:37:25Z
updated_at: 2024-10-04T15:53:49Z
url: https://github.com/astral-sh/ruff/issues/13625
synced_at: 2026-01-07T13:12:15-06:00
---

# C416 incorrectly flags dict comprehension based on dict with tuple keys

---

_Issue opened by @henribru on 2024-10-04 14:37_


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://play.ruff.rs/4d78b865-9778-415b-9303-377a3fdff673

```python
d1 = {(1, 2): 3, (3, 4): 5}
d2 = {x: y for x, y in d1}
```

[C416](https://docs.astral.sh/ruff/rules/unnecessary-comprehension/) suggests changing `d2` to `dict(d1)`, which is incorrect because this will just copy `d1`. 

This seems like an intractable problem to solve in general because you'd have to know whether the thing being iterated over is a dict. In a case like this I suppose it could be done with local type inference, though. [SIM118](https://docs.astral.sh/ruff/rules/in-dict-keys/) mentions acting differently based on whether it can statically know something to be a dict, so I assume that's something Ruff is already capable of.

The fix is thankfully already marked as unsafe for a different reason, so this isn't a huge problem. But it seems worth documenting this case as another reason for marking it as unsafe. 

---

_Comment by @zanieb on 2024-10-04 15:24_

Ah the classic case where the dict keys are tuples. I think we could do better here with type inference, yeah.

---

_Label `rule` added by @zanieb on 2024-10-04 15:24_

---

_Label `type-inference` added by @zanieb on 2024-10-04 15:24_

---

_Comment by @zanieb on 2024-10-04 15:26_

This is sort of a false positive of the rule not a fix safety problem though.

---

_Referenced in [astral-sh/ruff#13627](../../astral-sh/ruff/pulls/13627.md) on 2024-10-04 15:29_

---

_Comment by @henribru on 2024-10-04 15:53_

> This is sort of a false positive of the rule not a fix safety problem though.

Well, sure, but in general the only way to avoid false positives for this is to only flag it if you can statically determine that it's not a dict, right? But that would make the rule apply much more rarely, so the more pragmatic solution seems like keeping the fix as unsafe and avoiding flagging it if you can statically determine that it's a dict. Not sure if Ruff generally prefers correctness or more broadly applicable rules, though.

---

_Referenced in [astral-sh/ruff#18346](../../astral-sh/ruff/issues/18346.md) on 2025-05-28 07:50_

---
