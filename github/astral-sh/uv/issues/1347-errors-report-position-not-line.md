---
number: 1347
title: "Errors report \"position\" not line"
type: issue
state: closed
author: emsi
labels:
  - error messages
assignees: []
created_at: 2024-02-15T21:15:09Z
updated_at: 2024-02-27T12:34:59Z
url: https://github.com/astral-sh/uv/issues/1347
synced_at: 2026-01-07T13:12:16-06:00
---

# Errors report "position" not line

---

_Issue opened by @emsi on 2024-02-15 21:15_

When reporting errors `uv`  provides a position rather than line numer which makes it harder to debug:

```
# uv pip install -r requirements.txt                                                                                   
error: Unsupported requirement in requirements.txt at position 227     
```

---

_Label `error messages` added by @charliermarsh on 2024-02-15 21:16_

---

_Comment by @MichaReiser on 2024-02-27 12:34_

I merge this into https://github.com/astral-sh/uv/issues/2012

---

_Closed by @MichaReiser on 2024-02-27 12:34_

---
