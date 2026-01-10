```yaml
number: 1913
title: "UP011 is incorrectly replacing `lru_cache(max_size)=None`"
type: issue
state: closed
author: henribru
labels:
  - bug
assignees: []
created_at: 2023-01-16T11:34:15Z
updated_at: 2023-01-17T21:57:09Z
url: https://github.com/astral-sh/ruff/issues/1913
synced_at: 2026-01-10T11:09:44Z
```

# UP011 is incorrectly replacing `lru_cache(max_size)=None`

---

_Issue opened by @henribru on 2023-01-16 11:34_

The UP011 rule replaces `functools.lru_cache(max_size=None)` with `functools.lru_cache`. The corresponding rule in pyupgrade replaces it with `functools.cache`: https://github.com/asottile/pyupgrade#replace-functoolslru_cachemaxsizenone-with-shorthand. 

Tested with ruff 0.0.222

---

_Label `bug` added by @charliermarsh on 2023-01-16 16:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-16 17:27_

---

_Comment by @charliermarsh on 2023-01-16 17:35_

Thank you! Fixing now. Just an oversight.

---

_Closed by @charliermarsh on 2023-01-16 18:14_

---

_Closed by @charliermarsh on 2023-01-16 18:14_

---

_Comment by @henribru on 2023-01-17 09:48_

@charliermarsh Thanks for the super quick response! Your fix seems to work fine for the `@functools.lru_cache(max_size=None)` case, but there's still a slight issue if you do `from functools import lru_cache` and then `@lru_cache(max_size=None)`. In that case it doesn't auto-fix anything, but it still suggests "Remove unnecessary parameters"

---

_Comment by @charliermarsh on 2023-01-17 12:39_

@henribru - Yeah this is a known limitation (and pyupgrade doesn't fix that case either, IIRC). In that case, we actually need to modify the _import_ too, to include `cache`. So for now, we flag it, but can't fix it.

Another case of #835.

---

_Comment by @henribru on 2023-01-17 12:55_

@charliermarsh That's a fair limitation, but still the message is pretty misleading since it's telling you to remove the parameters instead of changing `lru_cache` to `cache`. Since just removing `max_size=None` changes the behavior, it's not "unnecessary"

---

_Comment by @henribru on 2023-01-17 13:00_

Note that pyupgrade separates this into two rules, one that converts `lru_cache()` into `lru_cache` and one that converts `lru_cache(max_size=None)` into `cache`. The current error message works for the first case, but it seems misleading for the second

---

_Comment by @charliermarsh on 2023-01-17 13:01_

Ah right -- I'll modify the error message.

---

_Comment by @charliermarsh on 2023-01-17 13:02_

(And consider splitting into two rules, too.)

---

_Comment by @charliermarsh on 2023-01-17 20:08_

Fixed in #1938.

---

_Comment by @henribru on 2023-01-17 21:54_

Thanks again for all your hard work, I really appreciate it!

---

_Comment by @charliermarsh on 2023-01-17 21:57_

Thanks for being persistent on this one even after I closed it out -- you were totally right! 

---
