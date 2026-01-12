```yaml
number: 15998
title: "`DTZ005`: false positive when `replace(...)` before `astimezone()`"
type: issue
state: closed
author: trim21
labels:
  - bug
assignees: []
created_at: 2025-02-06T16:15:33Z
updated_at: 2025-02-09T15:49:00Z
url: https://github.com/astral-sh/ruff/issues/15998
synced_at: 2026-01-12T15:54:55Z
```

# `DTZ005`: false positive when `replace(...)` before `astimezone()`

---

_@trim21_

### Description

```python
import datetime


print("update at", datetime.datetime.now().replace(microsecond=0).astimezone())
```

https://play.ruff.rs/ca289032-db85-4067-bbf2-e77e59d90365

---

_Renamed from "`DTZ005`: false positive with `replace`" to "`DTZ005`: false positive when `replace(...)` before `astimezone()`" by @trim21 on 2025-02-06 16:16_

---

_Label `bug` added by @ntBre on 2025-02-06 20:08_

---

_Comment by @ntBre on 2025-02-06 20:09_

Thanks for the report! I think this can be considered a bug because we do have special handling for [astimezone](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_datetimez/rules/call_datetime_now_without_tzinfo.rs#L85) that could possibly be extended to other known methods.

---

_Comment by @ayushbaweja on 2025-02-08 06:30_

Should DTZ005 only allow replace() method chains before astimezone(), or should it allow any method/attribute chain as long as the object remains a datetime.datetime before calling astimezone()?

For example, would both of these be valid?
```
datetime.now().replace(hour=12).astimezone()  
datetime.now().other_method().foo.bar().astimezone()  # Using other methods/attributes that return datetime object
```

---

_Comment by @trim21 on 2025-02-08 09:50_

I think `replace` is the only method on datatime return a datatime except `astimezone`？ `.other_method().foo.bar()` currently doesn't exist.

---

_Closed by @AlexWaygood on 2025-02-09 15:49_

---
