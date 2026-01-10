---
number: 13784
title: Walltime benchmark jobs take 2x as long as instrumented ones
type: issue
state: open
author: zanieb
labels:
  - internal
assignees: []
created_at: 2025-06-02T13:43:44Z
updated_at: 2025-06-02T13:49:44Z
url: https://github.com/astral-sh/uv/issues/13784
synced_at: 2026-01-10T01:25:38Z
---

# Walltime benchmark jobs take 2x as long as instrumented ones

---

_Issue opened by @zanieb on 2025-06-02 13:43_

We pay per minute for the walltime runs, so we should try to identify why these are slower and speed them up.

e.g., in https://github.com/astral-sh/uv/actions/runs/15392484763/job/43304959523

we see 14m vs 7m. +5m during _setup_, running the benchmarks is about the same.

---

_Label `internal` added by @zanieb on 2025-06-02 13:43_

---

_Renamed from "Walltime benchmarks take 2x as long as instrumented ones" to "Walltime benchmark jobs take 2x as long as instrumented ones" by @zanieb on 2025-06-02 13:46_

---

_Comment by @zanieb on 2025-06-02 13:49_

This might be mostly during the

```
sudo apt-get update
sudo apt-get install -y libsasl2-dev libldap2-dev libkrb5-dev
```

steps?

The cargo build looks ~1m slower too.

---
