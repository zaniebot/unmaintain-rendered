```yaml
number: 21327
title: Enable perf profile upload for walltime benchmarks
type: pull_request
state: closed
author: MichaReiser
labels:
  - ci
assignees: []
base: main
head: micha/wall-time-perf
created_at: 2025-11-07T19:41:33Z
updated_at: 2025-11-16T11:46:49Z
url: https://github.com/astral-sh/ruff/pull/21327
synced_at: 2026-01-10T16:53:55Z
```

# Enable perf profile upload for walltime benchmarks

---

_Pull request opened by @MichaReiser on 2025-11-07 19:41_

Enable the walltime performance profile upload for codspeed walltime benchmarks. 

This does increase the walltime benchmark runtime by ~2min (from ~8 to ~10 and ~9 to ~11min). I do find the profile's useful when investigating a regression (to get a first impression of where it might be coming from). 

---

_Label `ci` added by @MichaReiser on 2025-11-07 19:41_

---

_Comment by @github-actions[bot] on 2025-11-07 20:02_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-11-07 20:41_

---

_Renamed from "Experiment with enabling walltime perf profile upload" to "Enable perf profile upload for walltime benchmarks" by @MichaReiser on 2025-11-09 15:31_

---

_@AlexWaygood reviewed on 2025-11-10 12:21_

I feel like we must have had very different experiences when investigating ty performance regressions 😆

To me, the benefits still don't seem worth the cost here. I've found the codspeed flamegraphs very useful when investigating Ruff performance regressions, but I can't recall a time when I've found they've given me any useful information regarding a ty performance regression -- and all our walltime benchmarks are ty benchmarks. A 2-minute increase in CI times is a pretty significant increase, meanwhile.

---

_Closed by @MichaReiser on 2025-11-12 07:45_

---

_Reopened by @MichaReiser on 2025-11-12 07:46_

---

_Comment by @MichaReiser on 2025-11-12 08:01_

I used my watch to stop the time, it's 1min and 20s. I agree, that's somewhat significant. 

> I've found they've given me any useful information regarding a ty performance regression -- and all our walltime benchmarks are ty benchmarks. A 2-minute increase in CI times is a pretty significant increase, meanwhile.

I agree that they're less useful but they often give me at least an idea where a regression comes from when reviewing another PR without having to clone it and do some local profiling. 

---

_Closed by @MichaReiser on 2025-11-12 08:01_

---

_Branch deleted on 2025-11-16 11:46_

---
