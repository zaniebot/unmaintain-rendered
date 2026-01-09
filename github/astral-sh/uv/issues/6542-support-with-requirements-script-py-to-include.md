---
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
synced_at: 2026-01-07T13:12:17-06:00
---

# Support `--with-requirements script.py` to include inline dependency metadata from another script

---

_Issue opened by @zanieb on 2024-08-23 19:41_

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

_Referenced in [astral-sh/uv#6805](../../astral-sh/uv/issues/6805.md) on 2024-08-29 13:44_

---

_Referenced in [astral-sh/uv#7032](../../astral-sh/uv/issues/7032.md) on 2024-09-04 16:14_

---

_Referenced in [astral-sh/uv#11472](../../astral-sh/uv/issues/11472.md) on 2025-02-13 10:10_

---

_Referenced in [astral-sh/uv#11623](../../astral-sh/uv/pulls/11623.md) on 2025-02-19 15:49_

---

_Referenced in [astral-sh/uv#11792](../../astral-sh/uv/issues/11792.md) on 2025-02-26 15:35_

---

_Referenced in [astral-sh/uv#12071](../../astral-sh/uv/issues/12071.md) on 2025-03-09 15:32_

---

_Referenced in [astral-sh/uv#12636](../../astral-sh/uv/issues/12636.md) on 2025-04-02 19:22_

---

_Label `wish` removed by @zanieb on 2025-04-08 14:43_

---

_Label `help wanted` added by @zanieb on 2025-04-08 14:43_

---

_Comment by @zanieb on 2025-04-08 15:02_

I'd welcome a PR adding support here, if anyone is interested.

---

_Referenced in [astral-sh/uv#12763](../../astral-sh/uv/pulls/12763.md) on 2025-04-08 21:08_

---

_Comment by @thelastnode on 2025-06-19 21:21_

It sounds like this would allow support for inline pytests with something like: `uvx --with-requirements script.py pytest script.py`

This would be very useful for LLM-produced micro-scripts!

---

_Referenced in [astral-sh/ty#691](../../astral-sh/ty/issues/691.md) on 2025-06-23 06:03_

---

_Referenced in [astral-sh/uv#15296](../../astral-sh/uv/issues/15296.md) on 2025-08-15 08:08_

---

_Comment by @purepani on 2025-09-05 16:41_

I would absolutely love `--with-requirements` to wrap lsps for and get completion in PEP 723!

---

_Closed by @zanieb on 2025-09-05 16:45_

---

_Referenced in [pegasystems/pega-datascientist-tools#468](../../pegasystems/pega-datascientist-tools/pulls/468.md) on 2025-11-21 15:02_

---
