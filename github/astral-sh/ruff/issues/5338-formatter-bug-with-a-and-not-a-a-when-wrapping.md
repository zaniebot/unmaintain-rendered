---
number: 5338
title: "formatter bug with `a and not a & a` when wrapping"
type: issue
state: closed
author: davidszotten
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-06-23T16:31:45Z
updated_at: 2023-06-29T06:09:28Z
url: https://github.com/astral-sh/ruff/issues/5338
synced_at: 2026-01-07T13:12:15-06:00
---

# formatter bug with `a and not a & a` when wrapping

---

_Issue opened by @davidszotten on 2023-06-23 16:31_

In

```
if a and not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa & aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
    ...
```

out

```
if a and not aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
& aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa:
    ...
```

(note missing brackets)

---

_Label `formatter` added by @konstin on 2023-06-23 16:44_

---

_Label `bug` added by @MichaReiser on 2023-06-23 16:59_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-06-23 16:59_

---

_Referenced in [astral-sh/ruff#5370](../../astral-sh/ruff/pulls/5370.md) on 2023-06-26 15:19_

---

_Closed by @MichaReiser on 2023-06-29 06:09_

---
