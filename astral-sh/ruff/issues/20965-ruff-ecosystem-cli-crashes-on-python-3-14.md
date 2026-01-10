```yaml
number: 20965
title: "`ruff-ecosystem` CLI crashes on Python 3.14"
type: issue
state: closed
author: dylwil3
labels:
  - bug
  - internal
assignees: []
created_at: 2025-10-19T04:09:42Z
updated_at: 2025-10-29T21:37:40Z
url: https://github.com/astral-sh/ruff/issues/20965
synced_at: 2026-01-10T11:10:00Z
```

# `ruff-ecosystem` CLI crashes on Python 3.14

---

_Issue opened by @dylwil3 on 2025-10-19 04:09_

This is not a big deal since I can just install the tool with 3.13, but this line 

https://github.com/astral-sh/ruff/blob/3c229ae58abbe87523c9a2d3223ebcc67962137b/python/ruff-ecosystem/ruff_ecosystem/cli.py#L90

causes a crash on Python 3.14 when there is not yet an event loop (see https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.get_event_loop). 

---

_Label `bug` added by @dylwil3 on 2025-10-19 04:09_

---

_Label `internal` added by @dylwil3 on 2025-10-19 04:09_

---

_Closed by @MichaReiser on 2025-10-29 21:37_

---
