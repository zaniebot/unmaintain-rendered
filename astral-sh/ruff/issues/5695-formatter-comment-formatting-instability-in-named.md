---
number: 5695
title: "formatter: comment formatting instability in named expr (and dict, others)"
type: issue
state: closed
author: davidszotten
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-07-11T20:09:27Z
updated_at: 2023-08-18T03:10:47Z
url: https://github.com/astral-sh/ruff/issues/5695
synced_at: 2026-01-10T01:22:44Z
---

# formatter: comment formatting instability in named expr (and dict, others)

---

_Issue opened by @davidszotten on 2023-07-11 20:09_

input

```
if (
    # 1
    x # 2
    # 2.5
    := # 3
    # 3.5
    y # 4
):
    pass
```

```
Formatted once:
---
if (
    # 1
    x := # 2.5  # 2  # 3
    # 3.5
    y  # 4
):
    pass

---

Formatted twice:
---
if (
    # 1
    x := # 3.5  # 2.5  # 2  # 3
    y  # 4
):
    pass
```

i think the issue is that we use a `space()` around the operator (`:=`) and not a `soft_line_break_or_space` so nothing is inserting the newline before the leading ownline comment

---

_Comment by @MichaReiser on 2023-07-11 20:26_

The use of space might be intentional to avoid breaking after the operator. But it might be necessary to emit a soft line break if the right side has a leading comment 

---

_Label `bug` added by @MichaReiser on 2023-07-11 20:26_

---

_Label `formatter` added by @MichaReiser on 2023-07-11 20:26_

---

_Comment by @davidszotten on 2023-07-11 20:49_

yes sorry for being unclear. wasn't suggesting switching out the `space`, only thinking out loud why we hadn't seen this more elsewhere

---

_Referenced in [astral-sh/ruff#5705](../../astral-sh/ruff/pulls/5705.md) on 2023-07-12 08:50_

---

_Renamed from "formatter: bug in named expr (and dict, others)" to "formatter: comment formatting instability in named expr (and dict, others)" by @konstin on 2023-07-16 18:14_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-16 22:00_

---

_Comment by @charliermarsh on 2023-08-16 22:01_

I'll take this one. There are similarities to `with... as...` so I'll wait for that to be approved before pushing anything.

---

_Referenced in [astral-sh/ruff#6634](../../astral-sh/ruff/pulls/6634.md) on 2023-08-16 22:24_

---

_Comment by @charliermarsh on 2023-08-16 23:48_

Didn't end up being that similar to `with` so PR here: https://github.com/astral-sh/ruff/pull/6634.

---

_Closed by @charliermarsh on 2023-08-18 03:10_

---
