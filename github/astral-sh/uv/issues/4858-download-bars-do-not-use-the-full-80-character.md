---
number: 4858
title: Download bars do not use the full 80 character width
type: issue
state: open
author: zanieb
labels:
  - tracing
  - cli
assignees: []
created_at: 2024-07-07T17:05:14Z
updated_at: 2024-07-07T20:24:58Z
url: https://github.com/astral-sh/uv/issues/4858
synced_at: 2026-01-07T13:12:17-06:00
---

# Download bars do not use the full 80 character width

---

_Issue opened by @zanieb on 2024-07-07 17:05_

e.g. in

```
jsonpatch  ------------------------------     0 B/12.90 kB
annotated-types ------------------------------     0 B/13.64 kB
aiosqlite  ------------------------------     0 B/15.56 kB
itsdangerous ------------------------------     0 B/16.23 kB
toml       ------------------------------     0 B/16.59 kB
requests   ------------------------------ 48.01 kB/64.93 kB
idna       ------------------------------ 50.66 kB/66.84 kB
python-dateutil ------------------------------ 193.45 kB/229.89 kB
alembic    ------------------------------ 188.19 kB/232.99 kB
rich       ------------------------------ 180.22 kB/240.68 kB
orjson     ------------------------------ 196.61 kB/250.63 kB
greenlet   ------------------------------ 212.99 kB/273.08 kB
regex      ------------------------------ 212.99 kB/278.54 kB
```

we allocate some space for the package names for alignment but we're only at 67 total characters in the longest line — we should probably make use of at least some of the remaining 13 characters of space for package names to keep things aligned more often.

---

_Label `cli` added by @zanieb on 2024-07-07 17:05_

---

_Label `tracing` added by @zanieb on 2024-07-07 17:05_

---

_Comment by @charliermarsh on 2024-07-07 20:19_

Do you mean 80 here as in a conventional limit? Or does 80 have other significance?

---

_Comment by @zanieb on 2024-07-07 20:24_

Here I'm just referring to 80 as the conventional terminal width, yep.

---
