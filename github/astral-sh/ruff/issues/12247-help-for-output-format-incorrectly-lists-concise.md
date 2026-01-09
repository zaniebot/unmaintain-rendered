---
number: 12247
title: "help for \"--output-format\" incorrectly lists \"concise\" as the default"
type: issue
state: closed
author: DaniBodor
labels:
  - good first issue
  - cli
assignees: []
created_at: 2024-07-08T16:48:18Z
updated_at: 2024-07-09T02:45:25Z
url: https://github.com/astral-sh/ruff/issues/12247
synced_at: 2026-01-07T13:12:15-06:00
---

# help for "--output-format" incorrectly lists "concise" as the default

---

_Issue opened by @DaniBodor on 2024-07-08 16:48_

I've noticed that as of v0.5.0 the default output format has changed from "concise" to "full". However, the help still states that the default is "concise".

Minimal reproduction:

```
ruff check -h
```

returns:

> Run Ruff on the given files or directories (default)
> 
> [yadda yadda]
> 
> --output-format <OUTPUT_FORMAT>    Output serialization format for violations. The default serialization format is "concise". In preview mode,
> 
> [yadda yadda]



---

_Comment by @charliermarsh on 2024-07-08 16:50_

Thank you! PR welcome if anyone is up for it.

---

_Label `good first issue` added by @zanieb on 2024-07-08 16:51_

---

_Label `cli` added by @zanieb on 2024-07-08 16:51_

---

_Referenced in [astral-sh/ruff#12248](../../astral-sh/ruff/pulls/12248.md) on 2024-07-08 17:08_

---

_Comment by @DaniBodor on 2024-07-08 17:09_

> Thank you! PR welcome if anyone is up for it.

done

---

_Closed by @charliermarsh on 2024-07-09 02:45_

---

_Closed by @charliermarsh on 2024-07-09 02:45_

---
