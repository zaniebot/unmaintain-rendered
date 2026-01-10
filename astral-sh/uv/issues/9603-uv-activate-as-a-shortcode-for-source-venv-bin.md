---
number: 9603
title: "uv activate as a shortcode for `source .venv/bin/activate`"
type: issue
state: closed
author: dmi3kno
labels:
  - duplicate
assignees: []
created_at: 2024-12-03T11:09:56Z
updated_at: 2024-12-03T16:13:06Z
url: https://github.com/astral-sh/uv/issues/9603
synced_at: 2026-01-10T01:24:43Z
---

# uv activate as a shortcode for `source .venv/bin/activate`

---

_Issue opened by @dmi3kno on 2024-12-03 11:09_

I would appreciate a short command which could abstract the environment activation

```{bash}
uv activate [environment]
```
which would by default (without the environment specified) resolve to

```{bash}
source .venv/bin/activate
```

This would simplify the workflow and make `uv` easier to adopt in teaching.

---

_Comment by @my1e5 on 2024-12-03 11:22_

Duplicate of many
* https://github.com/astral-sh/uv/issues/8106
* https://github.com/astral-sh/uv/issues/8118
* https://github.com/astral-sh/uv/issues/9166


---

_Comment by @dmi3kno on 2024-12-03 11:29_

Oh well, very sorry about it.

---

_Closed by @dmi3kno on 2024-12-03 11:29_

---

_Label `duplicate` added by @zanieb on 2024-12-03 15:15_

---

_Comment by @zanieb on 2024-12-03 15:15_

Please read #9452 next time.

---

_Comment by @dmi3kno on 2024-12-03 16:06_

I apologized. And I am sorry again. Please forgive me. I closed the issue within 15 min I think. I did do a search but failed to find it. **I am sorry**

---

_Comment by @zanieb on 2024-12-03 16:13_

It's not a big deal, duplicates happen all the time. I'm referring you to #9452 as a resource — it includes a link to this specific request _because_ GitHub search frequently fails.

---
