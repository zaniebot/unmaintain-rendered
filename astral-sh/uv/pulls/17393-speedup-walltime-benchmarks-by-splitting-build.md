```yaml
number: 17393
title: Speedup walltime benchmarks by splitting build and run jobs
type: pull_request
state: open
author: zanieb
labels:
  - internal
assignees: []
base: main
head: claude/port-ruff-22126-Gz4vw
created_at: 2026-01-09T21:34:27Z
updated_at: 2026-01-12T23:30:09Z
url: https://github.com/astral-sh/uv/pull/17393
synced_at: 2026-01-13T00:22:58Z
```

# Speedup walltime benchmarks by splitting build and run jobs

---

_@zanieb_

Copies astral-sh/ruff#22126

We can use a larger _and_ less expensive runner for the build step.

Total runtime went from 18m -> 15m and most of that time is no longer on the benchmark runner.

---

_Marked ready for review by @zanieb on 2026-01-12 20:53_

---

_Label `internal` added by @zanieb on 2026-01-12 20:55_

---
