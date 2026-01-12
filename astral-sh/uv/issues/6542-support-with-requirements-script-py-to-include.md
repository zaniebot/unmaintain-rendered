```yaml
number: 6542
title: "Support `--with-requirements script.py` to include inline dependency metadata from another script"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-08-23T19:41:31Z
updated_at: 2025-09-05T16:45:47Z
url: https://github.com/astral-sh/uv/issues/6542
synced_at: 2026-01-12T15:59:05Z
```

# Support `--with-requirements script.py` to include inline dependency metadata from another script

---

_@zanieb_

I'm not sure we should support this, but to track the idea... 

`uvx --with-requirements script.py <tool> script.py` would be a cool way to give access to the requirements of a script.

---

_Label `wish` added by @zanieb on 2024-08-23 19:41_

---

_Label `cli` added by @zanieb on 2024-08-23 19:41_

---

_Comment by @zanieb on 2024-08-23 19:42_

Basically this is an alternative for calling the script with something other than `python`.

Maybe it's just

```
uv run --script-runner <foo> script.py
```

---

_Label `wish` removed by @zanieb on 2025-04-08 14:43_

---

_Label `help wanted` added by @zanieb on 2025-04-08 14:43_

---

_Comment by @zanieb on 2025-04-08 15:02_

I'd welcome a PR adding support here, if anyone is interested.

---

_Comment by @thelastnode on 2025-06-19 21:21_

It sounds like this would allow support for inline pytests with something like: `uvx --with-requirements script.py pytest script.py`

This would be very useful for LLM-produced micro-scripts!

---

_Comment by @purepani on 2025-09-05 16:41_

I would absolutely love `--with-requirements` to wrap lsps for and get completion in PEP 723!

---

_Closed by @zanieb on 2025-09-05 16:45_

---
