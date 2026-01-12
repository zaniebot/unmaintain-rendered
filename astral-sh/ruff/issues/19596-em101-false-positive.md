```yaml
number: 19596
title: EM101 false positive
type: issue
state: closed
author: aspizu
labels:
  - bug
  - rule
assignees: []
created_at: 2025-07-28T12:37:09Z
updated_at: 2025-08-01T17:37:45Z
url: https://github.com/astral-sh/ruff/issues/19596
synced_at: 2026-01-12T15:54:56Z
```

# EM101 false positive

---

_@aspizu_

### Summary

<https://play.ruff.rs/a6f026e4-bf4e-487f-89f2-dbfb25d991ac>

```py
import typing

raise typing.cast("Exception", None)
```

The fix for EM101 is applied.

which becomes:
```py
import typing

msg = "Exception"
raise typing.cast(msg, None)
```

then the fix for `TC006` is applied.

which becomes:
```py
import typing

msg = "Exception"
raise typing.cast("msg", None)
```

and it becomes a loop, since that introduces `EM101` again.


### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Label `bug` added by @MichaReiser on 2025-07-28 12:50_

---

_Label `rule` added by @MichaReiser on 2025-07-28 12:50_

---

_Comment by @MichaReiser on 2025-07-28 12:51_

Thanks. I think it makes sense to exclude `typing.cast` from `EM101` (I'm a bit surprised that we flag any function call and not just calls to classes (variables starting with an uppercase character)

---

_Closed by @ntBre on 2025-08-01 17:37_

---
