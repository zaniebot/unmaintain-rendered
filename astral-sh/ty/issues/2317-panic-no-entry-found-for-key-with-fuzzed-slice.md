```yaml
number: 2317
title: "panic: `no entry found for key` with fuzzed slice expression"
type: issue
state: open
author: correctmost
labels:
  - fatal
assignees: []
created_at: 2026-01-03T17:21:34Z
updated_at: 2026-01-08T18:10:04Z
url: https://github.com/astral-sh/ty/issues/2317
synced_at: 2026-01-10T01:56:41Z
```

# panic: `no entry found for key` with fuzzed slice expression

---

_Issue opened by @correctmost on 2026-01-03 17:21_

### Summary

ty crashes when checking this fuzzed code:

```python
arr[::[x for *b in y for (b: _
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/semantic_index/ast_ids.rs:35:22 when checking `/home/user/a.py`: `no entry found for key`
```

### Version

ty 0.0.8

---

_Label `fatal` added by @MichaReiser on 2026-01-03 17:29_

---

_Comment by @MichaReiser on 2026-01-03 17:30_

@AlexWaygood saw a similar panic while typing in an `except` block. Might be worth double checking if, whatever the issue is here, isn't an issue for `except`.

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-03 17:30_

---

_Comment by @nemowang2003 on 2026-01-07 05:42_

> [@AlexWaygood](https://github.com/AlexWaygood) saw a similar panic while typing in an `except` block.

Yes, I also encountered that this panic, and is consistently reproducible in an `except` block. 

Minimal reproduction:
```python
try:
    print()
except # Trigger completion/hover here
```

> Error: panicked at crates/ty_python_semantic/src/semantic_model.rs:576:1 no entry found for keyquery stacktrace

Hope this helps!

---

_Comment by @carljm on 2026-01-08 18:08_

It's not clear to @AlexWaygood and I that the `except` bug and the OP bug here are actually the same issue -- there can be totally separate underlying causes for the same "no entry found for key" symptom.

---

_Comment by @carljm on 2026-01-08 18:10_

Filed #2401 to track the `except` issue separately.

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-08 18:10_

---

_Added to milestone `Stable` by @carljm on 2026-01-08 18:10_

---
