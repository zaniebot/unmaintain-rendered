```yaml
number: 2250
title: PEP8 naming error raised when re-assigning to global
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-01-27T13:15:54Z
updated_at: 2023-01-28T13:40:11Z
url: https://github.com/astral-sh/ruff/issues/2250
synced_at: 2026-01-10T11:09:45Z
```

# PEP8 naming error raised when re-assigning to global

---

_Issue opened by @charliermarsh on 2023-01-27 13:15_

```py
MAX_PORT = 8100
START_PORT = 8088
CURRENT_PORT = START_PORT


def setup_comm(rank, world_size):
    global CURRENT_PORT

    CURRENT_PORT += 1
    if CURRENT_PORT > MAX_PORT:
        CURRENT_PORT = START_PORT  # <- N806 Variable `CURRENT_PORT` in function should be lowercase
```

_Originally posted by @Borda in https://github.com/charliermarsh/ruff/issues/799#issuecomment-1406490812_
            

---

_Renamed from "@charliermarsh I see the same issue with `v0.0.233`" to "PEP8 naming error raised when re-assigning to global" by @charliermarsh on 2023-01-27 13:16_

---

_Closed by @charliermarsh on 2023-01-28 13:40_

---
