```yaml
number: 125
title: validate incremental correctness by checking successive commits of real projects
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-04-15T21:25:46Z
updated_at: 2025-11-18T16:10:23Z
url: https://github.com/astral-sh/ty/issues/125
synced_at: 2026-01-10T01:58:59Z
```

# validate incremental correctness by checking successive commits of real projects

---

_Issue opened by @carljm on 2025-04-15 21:25_

We should increase confidence in the correctness of our incremental checking by taking real projects (e.g. mypy-primer projects) and running red-knot incrementally on successive commits of those projects, then validating that the resulting diagnostics are identical to running a cold check on each of those commits.

---

_Comment by @MichaReiser on 2025-04-22 08:28_

That's a great idea to verify if `--watch` works as expected. I don't think this is necessary to test Red Knot's core incrementality, our mdtests give us a fairly decent coverage there (they all run incrementally)

---

_Comment by @carljm on 2025-04-22 14:03_

Mdtests all run incrementally, but in a distinctive and limited way where all files are totally removed and then re-added for each test. There's a lot of potential bug surface where files are just modified in smaller ways. 

---

_Comment by @MichaReiser on 2025-04-22 14:10_

> Mdtests all run incrementally, but in a distinctive and limited way where all files are totally removed and then re-added for each test.

The `File` ingredients are reused if they map to the same path. So we do test that part. But it's definetely interesting to test this on larger projects with, that also have more cyclic dependencies.

---

_Renamed from "[red-knot] validate incremental correctness by checking successive commits of real projects" to "validate incremental correctness by checking successive commits of real projects" by @MichaReiser on 2025-05-07 15:25_

---

_Label `server` added by @AlexWaygood on 2025-05-11 07:38_

---

_Label `server` removed by @MichaReiser on 2025-05-19 07:10_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 00:59_

---

_Added to milestone `GA` by @carljm on 2025-06-11 00:59_

---

_Removed from milestone `Stable` by @carljm on 2025-11-13 00:56_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 14:32_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
