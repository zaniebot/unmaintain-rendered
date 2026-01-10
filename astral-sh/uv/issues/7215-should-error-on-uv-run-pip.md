---
number: 7215
title: "Should error on `uv run pip`?"
type: issue
state: closed
author: Kludex
labels:
  - question
assignees: []
created_at: 2024-09-09T11:54:39Z
updated_at: 2024-10-21T21:45:48Z
url: https://github.com/astral-sh/uv/issues/7215
synced_at: 2026-01-10T01:24:12Z
---

# Should error on `uv run pip`?

---

_Issue opened by @Kludex on 2024-09-09 11:54_

Hi there,

It took me some time to realize I was running `uv run pip` instead of `uv pip`.

I don't really know the difference, but when running `uv run` after what I did with `uv run pip` was removed from the `.venv`.

---

_Comment by @ChannyClaus on 2024-09-11 04:33_

isn't that intended? i believe the later invocation of `uv pip` / `pip` is expected to write over the current state of the virtual environment, e.g.

```
$ uv pip install pandas
Resolved 6 packages in 13ms
Installed 6 packages in 48ms
 + numpy==2.1.1
 + pandas==2.2.2
 + python-dateutil==2.9.0.post0
 + pytz==2024.2
 + six==1.16.0
 + tzdata==2024.1
```
followed by
```
$ uv pip install numpy~=1.0
Resolved 1 package in 10ms
Uninstalled 1 package in 41ms
Installed 1 package in 14ms
 - numpy==2.1.1
 + numpy==1.26.4
```
overrides the numpy version.

also given that `uv run` is designed to (someone correct me if i'm wrong here) support _any_ arbitrary command that follows, erring on a particular command (i.e. `pip`) feels like a hack.

in fact, i think there may be some use cases (albeit not ideal) where one wants to combine the use of `pip` and `uv pip` (e.g. if `pip` and `uv pip` behave differently in some configuration).

---

_Label `question` added by @zanieb on 2024-09-22 23:30_

---

_Comment by @zanieb on 2024-10-21 21:45_

Probably a duplicate of https://github.com/astral-sh/uv/issues/5839

---

_Closed by @zanieb on 2024-10-21 21:45_

---
