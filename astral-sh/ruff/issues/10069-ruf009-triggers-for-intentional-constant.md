---
number: 10069
title: RUF009 triggers for intentional constant computation
type: issue
state: closed
author: RonnyPfannschmidt
labels: []
assignees: []
created_at: 2024-02-21T10:00:06Z
updated_at: 2024-02-22T16:32:20Z
url: https://github.com/astral-sh/ruff/issues/10069
synced_at: 2026-01-10T01:22:49Z
---

# RUF009 triggers for intentional constant computation

---

_Issue opened by @RonnyPfannschmidt on 2024-02-21 10:00_

https://play.ruff.rs/aa67fe33-c09c-4a7f-91fc-5fe9b5b98425

in pytest i replaced a magic constant with its more comprehensible computation

```
# minimized reducers
import datetime
import dataclasses

@dataclasses.dataclass
class MockTiming:
   
    _current_time: float = datetime(2020, 5, 22, 14, 20, 50).timestamp()
    _current_time_before: float = 1590150050.0
```

i recon that its possibly hard to sanely filter false positives
i do wonder if given type annotation awareness it could permit in case the value is a builtin immutable 

---

_Comment by @MichaReiser on 2024-02-22 16:32_

Yeah, it might be possible but it isn't sufficient to only test if the result type is immutable, ruff would also need to know if the function is idempotent (does not depend on any global or system state) as well as all arguments passed to the call. 

I understand that the result here is undesired but getting this right requires more analysis than ruff is capable today. I recommend manually suppressing the rule for this case and documenting why it is safe (documenting this might also help future readers).

---

_Closed by @MichaReiser on 2024-02-22 16:32_

---
