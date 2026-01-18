```yaml
number: 2401
title: panic for incomplete except block
type: issue
state: open
author: carljm
labels:
  - fatal
assignees: []
created_at: 2026-01-08T18:09:38Z
updated_at: 2026-01-18T11:01:13Z
url: https://github.com/astral-sh/ty/issues/2401
synced_at: 2026-01-18T11:18:08Z
```

# panic for incomplete except block

---

_@carljm_

> [@AlexWaygood](https://github.com/AlexWaygood) saw a similar panic while typing in an `except` block.
> 
> Yes, I also encountered that this panic, and is consistently reproducible in an `except` block. 
> 
> Minimal reproduction:
> ```python
> try:
>     print()
> except # Trigger completion/hover here
> ```
> 
> Error: panicked at crates/ty_python_semantic/src/semantic_model.rs:576:1 no entry found for keyquery stacktrace
> 
> Hope this helps! 

 _Originally posted by @nemowang2003 in [#2317](https://github.com/astral-sh/ty/issues/2317#issuecomment-3717402403)_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 18:09_

---

_Label `fatal` added by @carljm on 2026-01-08 18:09_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 03:47_

---

_Comment by @AlexWaygood on 2026-01-18 11:01_

Another example from #2545:

```python
    async def _run(self) -> None:
        try:
            ...

        except | # My cursor was here

        except* Exception as exception_group:
            ...

            raise
```

With the same panic message 

---
