```yaml
number: 1485
title: Add troubleshooting section to benchmarks guide
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: benchmark-troubleshooting
created_at: 2024-02-16T12:37:00Z
updated_at: 2024-02-16T14:27:33Z
url: https://github.com/astral-sh/uv/pull/1485
synced_at: 2026-01-12T16:04:37Z
```

# Add troubleshooting section to benchmarks guide

---

_@MichaReiser_

## Summary

It took me a while to figure out why the benchmarks are extremelly flaky (and slow) on my machine. 
I documented my finding in the benchmarks.md file in case someone else runs into the same problems as I


---

_@BurntSushi reviewed on 2024-02-16 14:17_

---

_Review comment by @BurntSushi on `BENCHMARKS.md`:113 on 2024-02-16 14:17_

Could you add a qualifying word here to indicate that this is about the cold benchmarks and not the warm ones? The warm benchmarks shouldn't suffer from the same problem. Maybe just say, "in your cold benchmarks" or "benchmarks that aren't using a cache."

---

_@MichaReiser reviewed on 2024-02-16 14:21_

---

_Review comment by @MichaReiser on `BENCHMARKS.md`:113 on 2024-02-16 14:21_

Good point

---

_@zanieb approved on 2024-02-16 14:27_

---

_Merged by @zanieb on 2024-02-16 14:27_

---

_Closed by @zanieb on 2024-02-16 14:27_

---

_Branch deleted on 2024-02-16 14:27_

---
